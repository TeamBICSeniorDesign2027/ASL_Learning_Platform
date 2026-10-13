# Design D1: ASL Learning Assistant

Team: Braden Monnin, Connor Slutsky, Isaac Dowdy

**Goal:** give self-taught ASL learners English text and hand-shape feedback from a webcam, without storing their video data.

**Input:** live webcam video of the learner performing an ASL sign.
**Output:** English text translation of the sign, plus feedback on the user's formation of the sign.

**Overview:** This system is intended to help learners validate their ASL signs. It'll be a web application such that a user will form their signs to their webcam, which will send their signs to an AI/ML model to guess what those signs are, how confident it is in that guess, and determine the accuracy of the signs. In D1, we covered the ML Model componenet. D2 will add the remaining components, edge cases, requirements traceability, design rational, and peer review. 


# Header, conventions, and D2 overview. 
~~Your project title and goal statement on every diagram,~~ with each diagram's conventions explained: what boxes mean, what lines represent. ~~Then a two-minute overview for a reviewer with no context: what the system is, what D1 covered, and what D2 adds.~~ List known gaps and open questions here rather than letting reviewers find them.


# Finished components, the D2 diagrams. 
For every component not fully detailed in D1: 1) a data model (or the words "no persistent data"), 2) its behavior as an algorithm entry, state machine, or step list, and 3) its interface as endpoints, message formats, or signatures with errors. 
Revisit the D0 interfaces and revise any that grew too many parameters or leak internals.

## Component (External): Video Intake

### Overview
Video Intake is handled predominantly by existing frameworks, notably with the JS/HTML5 MediaDevices API. It will record and encode webcam footage of the user performing signs.

### Data Model
No persistent storage.

### Behavior
1. User enables webcam
2. Webcam begins recording
3. User performs signs
4. Webcam via direct function call sends video data to MediaPipe framework

### Interface
No inputs (besides enabled webcam). Returns JS MediaStream object (https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia).

## Component (External): Input Processing

### Overview

This component is predominantly external as it's handled by the MediaPipe framework. This component will take the video data from the Video Intake component, determine the landmarks of the user's signs from the video data, and construct an HTTP request to the backend for prediction.

### Data Model

No persistent data

### Behavior

1) Pass video footage to MediaPipe
2) Get landmarks from function
3) Construct request to be sent to model (with sign_window_id and landmarks)
4) Send request to ML Model component

### Interface

Inputs - JS MediaStream object

Outputs - 
```json
{
  "sign_window_id": 17,
  "face": [[[0.42, 0.53], [0.45, 0.10], "..."], [[0.47, 0.67], [0.67, 0.67], "..."], "..."],
  "left": [[[0.43, 0.54], [0.46, 0.20], "..."], [[0.46, 0.51], [0.44, 0.39], "..."], "..."],
  "right": [[[0.41, 0.52], [0.44, 0.50], "..."], [[0.42, 0.51], [0.45, 0.49], "..."], "..."],
}
```


## Component: Results Processing

### Scope

Results Processing runs after the ML Model has classified a completed sign. It takes one classification and returns one feedback payload. 

It covers three D0 components that we have collapsed into one:

- **ASL to Text Translation** (D0). Maps the class label to English text.
- **Determine Sign Accuracy** (D0). Scores how closely the performed hand shape matches the reference shape for the recognized sign.
- **Confidence Check** (D0). Decides whether the classification is trustworthy enough to show, or should be reported as not recognized, and reports its confidence.

### Data Model

**Persistent user data: none.** Nothing is written to disk. Landmarks are discarded after scoring.

**Static and read-only (built offline, never written at runtime):** a library of signs keyed by class label. Each entry holds English text and a reference template: 21 landmark points for the hand shape, in the right-hand orientation and scaled so the wrist to landmark 9 distance is 1.

### Interface

| ID | Direction | Operation | Format | Protocol | Errors |
| ---| ---       | ---       | ---    | ---      | ---    |
| R-IN | From ML Model (merges D0 I5 and I6) | `process(result: ClassificationResult) -> FeedbackPayload` | JSON in, JSON out | HTTP `POST /results` (proposed), or an in-memory call if co-located | Malformed JSON, or no `sign_window_id`, returns `400`, because the result cannot be matched to a sign. Any other invalid input returns a `FeedbackPayload` with `status` `error`. |
| R-OUT | To Send Feedback (revised D0 I9) | The `FeedbackPayload` returned by `process` | JSON | Same as R-IN | Delivery to the learner belongs to Send Feedback and is not designed here. |

`left` and `right` hold the landmarks for the whole sign window: one entry per frame, each frame 21 `[x, y]` pairs, or `null` if that hand never appeared.

**R-IN, success** (D1 `/predict` output, I dropped `face` and `body` here because our discussions of sign accuracy have been only for hand positioning, but we could expand to include them as well):

```json
{
  "sign_window_id": 17,
  "class": "C",
  "confidence": 0.92,
  "frame_size": [640, 480],
  "left": [[[0.41, 0.52], [0.44, 0.50], "..."], [[0.42, 0.51], [0.45, 0.49], "..."], "..."],
  "right": null
}
```

**R-IN, error** (sent by the Model Output Builder when the model returns a 500 or cannot be reached):

```json
{ "sign_window_id": 17, "error": "MODEL_ERROR", "message": "Internal Server Error" }
```

**R-OUT, `FeedbackPayload`:**

```json
{
  "sign_window_id": 17,
  "status": "recognized",
  "recognized_text": "C",
  "overall_confidence": 0.92,
  "shape": {
    "score": 0.78,
    "landmarks": [
      { "landmark": 0, "deviation": 0.02, "dx": 0.00, "dy": 0.01 },
      { "landmark": 4, "deviation": 0.31, "dx": 0.12, "dy": -0.05 },
      "... one entry per landmark, 21 in total"
    ]
  },
  "error_code": null
}
```

| `status` | When it is returned | `recognized_text` | `overall_confidence` | `shape` |
| --- | --- | --- | --- | --- |
| `recognized` | Confidence is at or above the threshold and the label is found | Text from `SignLibrary` | Model confidence | A score and 21 landmark entries, or `null` if scoring failed |
| `not_recognized` | Confidence is below the threshold | `null` | Model confidence | `null` |
| `error` | Model error, invalid input, or unknown label | `null` | Model confidence if known, else `null` | `null` |

`error_code` is `null` unless `status` is `error`. The codes are `INVALID_CLASSIFICATION` (class not a string, confidence not a number from 0 to 1, or bad `frame_size`), `UNKNOWN_LABEL` (class not in `SignLibrary`), and `MODEL_ERROR` (error envelope from the model).

`shape.score` is from 0 to 1. Each landmark entry gives `deviation`, its distance from the template in template units, and `dx` and `dy`, the move that would bring that landmark onto the template. Positive `dx` means move right, and positive `dy` means move down, as the learner sees the image. Landmark numbers follow the MediaPipe Hands order defined in the shape-comparison algorithm.

### Behavior

Steps:

1. **Receive.** Parse the JSON. A malformed body, or one without `sign_window_id`, gets a `400`, and nothing is scored.
2. **Model error.** If the body is an error envelope, return an `error` payload with `MODEL_ERROR` and stop.
3. **Validate.** `class` must be a string, `confidence` a number from 0 to 1, and `frame_size` two positive integers. Otherwise return `error` with `INVALID_CLASSIFICATION`.
4. **Confidence check.** If `confidence` is below `CONFIDENCE_THRESHOLD` (proposed 0.80, with equal values passing), return `not_recognized` with `overall_confidence` set to the model's confidence, and stop.
5. **Translate.** Look up `class` in sign library data. A missing label returns `error` with `UNKNOWN_LABEL`. Otherwise, `recognized_text` is the entry's `text`.
6. **Score.** Run the shape-comparison algorithm below on `left` and `right`, using `frame_size` and the entry's reference template. If it returns null, `shape` is `null` and the result stays `recognized`.
7. **Return.** Return a `recognized` payload with `overall_confidence` set to the model's confidence and `shape` from step 6.
Results Processing does not retry model calls. It only receives the model's output. Any retry of `/predict` is the caller's decision.

### Shape-comparison algorithm (Determine Sign Accuracy)

The scorer compares the performed hand with the class's reference template after removing position, scale, and rotation, so only the hand shape is compared. Input: `left` and `right` (each `null` or a list of frames, where each frame is 21 `[x, y]` pairs normalized to 0 to 1), `frame_size` `[W, H]`, and the recognized class label. Output: a shape score from 0 to 1 and 21 landmark entries, or `null`.

Landmarks follow the MediaPipe Hands order: 0 is the wrist, 1 to 4 the thumb, 5 to 8 the index finger, 9 to 12 the middle finger, 13 to 16 the ring finger, and 17 to 20 the pinky. Within each finger, the lower number is nearer the palm.

1. **Check the hands.** A frame is usable if it has exactly 21 pairs and every value is between 0 and 1. A hand is usable if at least one of its frames is usable. If neither hand is usable, return `null`
2 **Loop through hands.** If both hands are usable, run the algorithm for each. If only one is usable, run the following steps only once.
3. **Convert to pixels.** Multiply each point's x by `W` and its y by `H`. D1 normalizes x and y by different sizes, so without this step a non-square frame would distort the shape.
5. **Align and pick the best frame.** For each usable frame, find the rotation, uniform scale, and translation that best map its 21 points onto the template's 21 points (Procrustes analysis, least squares). A landmark's deviation is the distance between its mapped position and its template position. The frame with the smallest mean deviation is the best frame for the window.
6. **Corrections.** For the best frame, the correction for each landmark is the vector from its mapped position to its template position, so `dx` and `dy` are the template position minus the mapped position.
7. **Score.** Let the mean deviation be the average of the 21 deviations. The shape score is clamp(1 - mean deviation / `DEVIATION_TOLERANCE`, 0, 1), where `DEVIATION_TOLERANCE` is 0.25 (we will need to test this value and maybe change). A mean deviation of 0 scores 1.0, and a mean deviation at or above the tolerance scores 0.0.
8. **Return.** Return the shape score and the 21 landmark entries (`landmark`, `deviation`, `dx`, `dy`).

Signs that move are scored by shape, not motion. Each frame is compared on its own, and the best-matching frame is used, so a sign counts as matched if the performed hand reaches the template at some point in the window. The path between shapes is not scored, so two signs that differ only in movement can get the same scores. 

### Notes/Open questions

- Where Results Processing runs. If it runs in the learner's browser, `process` is an in-memory call. If it is hosted, R-IN is HTTP. 
- Two-handed signs. We may need a way to more elegantly handle these.
- The proposed values (threshold 0.80, deviation tolerance 0.25)


## Component: Send Feedback

### Scope

Send Feedback is the front-end user interface component responsible for presenting the final English translation, hand-shape corrections, or system warnings to the user. It takes in the in the output from Results Processing and translates the raw data into visual UI updates to the learner's screen. 

### Data Model

**Persistent user data: none.** The component only holds the local UI for the current sign_window_id being rendered.

### Interface

| ID | Direction | Operation | Format | Protocol | Errors |
| ---| ---       | ---       | ---    | ---      | ---    |
| F-IN | From Results Processing (revised D0 I9)| `render(payload: FeedbackPayload)` | JSON in | HTTP `POST /results` (proposed), or an in-memory call if co-located | Malformed JSON, missing `sign_window_id`, or missing `status` results in the payload being silently dropped to prevent crashing the video feed. |
| F-OUT | To User's Screen | UI Update | UI Elements | React state render | Render errors are caught by React error boundaries; a faulty frame drops silently, preserving the live webcam view. |


**F-IN, success** (recognized):


```json
{
  "sign_window_id": 17,
  "status": "recognized",
  "recognized_text": "C",
  "overall_confidence": 0.92,
  "shape": {
    "score": 0.78,
    "landmarks": [
      { "landmark": 0, "deviation": 0.02, "dx": 0.00, "dy": 0.01 },
      { "landmark": 4, "deviation": 0.31, "dx": 0.12, "dy": -0.05 },
      "... one entry per landmark, 21 in total"
    ]
  },
  "error_code": null
}
```

**F-IN, error** (error):

```json
{
  "sign_window_id": 19,
  "status": "error",
  "recognized_text": null,
  "overall_confidence": null,
  "shape": null,
  "error_code": "MODEL_ERROR"
}
```

**F-IN, low confidence** (not_recognized):

```json
{
  "sign_window_id": 18,
  "status": "not_recognized",
  "recognized_text": null,
  "overall_confidence": 0.45,
  "shape": null,
  "error_code": null
}
```

| `status` | When it is triggered | Text Overlay | Confidence Meter` | `shape` |
| --- | --- | --- | --- | --- |
| `recognized` | Valid sign recognized with high confidence | Displays `recognized_text` | Filled to percentage based on `overall_confidence` | Draws directional arrows for landmarks where deviation > visual threshold |
| `not_recognized` | Sign processed but falls below confidence threshold | "Sign not recognized. Please try again." | Filled to percentage based on overall_confidence | Cleared / Hidden |
| `error` | System, input, or model failure | User-friendly warning mapped from error_code | Hidden | Cleared / Hidden |

When drawing the Overlay, `dx` and `dy` from the shape.landmarks array dictate the direction of visual correction arrows rendered over the user's hand joints. A positive `dx` translates to a rightward-pointing arrow on the screen, and a positive `dy` translates to a downward-pointing arrow, directly guiding the learner to adjust their formation.

### Behavior

Steps:

1. **Receive.** The component listens for an incoming `FeedbackPayload`. If a new `sign_window_id` is received, clear the previous frame's visual feedback.
2. **Evaluate Status.** Parse the status field of the payload to determine the UI state. If `status` == `"error"`, map the `error_code` to a user-friendly warning banner. Stop processing. If `status` == `"not_recognized"`, display a subtle prompt asking the user to reposition or try again, and show the low overall_confidence score. Stop processing. If `status` == `"recognized"`, proceed.
3. **Render Text & Confidence.** Update the primary UI text overlay with the `recognized_text`. Update the confidence meter using the `overall_confidence`.
4. **Render Shape Feedback.** If `shape` is not null, extract the `shape.score` and display an accuracy rating. Then, iterate through the 21 entries in `shape.landmarks`. For any landmark where deviation exceeds a visual threshold, draw a directional indicator on the webcam canvas using `dx` and `dy`.
5. **Clear Stale State.** If no new payload is received within a timeout window, fade out the current feedback to prevent stale data from lingering on screen.

# Edge cases and error handling. 
One table with columns for ID, component, category, the edge case, and the design decision. Cover all four categories from class for each component: boundary values, timing and concurrency, bad or missing input, and external dependencies. Every row needs a decided behavior (retry, degrade, fall back, or surface an error), a named handler, and a note on whether it is recoverable. At least one diagram must show error paths, not only the happy path.

 - categories
   1) boundary values
   2) timing and concurrency
   3) bad or missing input
   4) external dependencies

| ID | Category | Edge Case | Decided Behavior | Handler | Recoverable (Y/N) |
| ---| ---      | ---       |  ---             | ---     | ---               |
|1|Bad/Missing Input|Hands not in frame|Display Error|Input Processing|N - have to retry|
|2|Timing/Concurrency|Users send request at same time|Queue requests|ML Model|Y|
|3|External Dependencies|Webcam not accessible / Disabled|Display Error|Video Intake|N - can't function without webcam|
|4|Boundary Values|Two guesses with confidence of 0.5|Arbitrarily pick the first value (e.g. first alphabetically)|Results Processing|Y|

**Note:** I'm not sure how the output of the model 100% works, whether it returns a ranking of of the top values it thinks the sign is, or if it only outputs one value. This assumes the ranking. 


## Edge Case Diagram?

do we have to do this?

# Requirements traceability table. 
One row per requirement, functional and non-functional: ID and short text, priority, the design elements that satisfy it 
(an entity, endpoint, algorithm, or figure, not a component name alone)
, and how. Explain every gap; never leave a cell blank. Run the check in reverse too, because a component with no requirement behind it is scope creep.  

| ID | Requirement | F/NF | Priority | Design Elements | How Addressed |
| ---| ---         | ---  |  ---     | --- | --- |
|1|Return prediction in less than 5 seconds|NF|High|Appropriate ML Model ????|We will implement model that can output a prediction in a timely manner|
|2|Be compatible with all fingerspelled letters of the alphabet and 50 signs in addition|F|High|Appropriate ML Model?|We will implement (a) model(s) that can determine the alphabet + 50 signs|
|3|Display error message and prompt reinput on improper input|F|High|Input Validation (Input Processing)|NaN values in landmarks will prompt error message and reinput|
|4|Be able to extract landmarks from webcam footage|F|High|MediaPipe|MediaPipe can determine facial and hand landmarks from images|
|5|Be able to accurately (~85%) predict signs that involve motion|NF|Medium|Input Processing and ML Model (???)|Input processing should handle a sequence of landmarks (to simulate movement), and the ML Model should be able to form a prediction based on a sequence of landmarks|
|6|Achieve overall accuracy of roughly 85%|NF|High|ML Model???|We will implmement an ML model that is accurate enough to classify signs correct 85% of the time|
|7|Display confidence and sign accuracy along with prediction|F|High|TODO|TODO|
|8|Display 'Processing...' icon when actively processing|F|Medium|UI|The UI will, on submission, display Processing... icon until a response is received from Results Processing|
|9|Be able to handle multiple users simultaneously|F|High|Queued requests|Queueing requests will allow the model to not be overwhelmed and predict sequentially, even on simultaneous inputs|
|10|Support both left and right-handed signers|F|High|ML Model???|We will implement a model that can accurately infer signs from right handed and left handed input|

# Design rationale. 
Six to 10 entries for the significant D1 and D2 decisions, each in the form 
"We chose X over Y because ___." 
"We accepted the trade-off of ___ because ___." 
Mark the decisions the peer review changed, and reference the D0 decision log rather than repeating it.

 1) We chose Python as a backend over C++ because Python has many accessible frameworks to develop AI/ML tools (including TensorFlow), and most of our team has experience with Python.
 2) We chose React JS over Vanilla JavaScript because it'll be more scalable in terms of the development of the user interface of our application.
 3) We chose TensorFlow as our Python AI/ML framework as one of our team members has experience with TensorFlow, it can handle larger models more efficiently, and can integrate with TFLite which allows for distribution of trained models. 
 4) We chose Azure as our final deployment of our app because more than one member of our team has experience using Azure.
 5) 5
 6) 6
 7) _7 maybe_
 8) _8 maybe_
 9) _9 maybe_
 10) _10 maybe_


# Peer design review notes. 
see docs/high_level_design/Peer_Review_Notes.md

# Suggestions
- Walk the D0 diagram box by box before class and assign what is missing to owners. That audit is faster than rediscovering the gaps in the review.
- Do not skip an edge-case row because the answer is hard. That row is where the reviewers will focus.
- Draw the decisions and failure outcomes of your riskiest flow, and a state machine for anything with a lifecycle.
