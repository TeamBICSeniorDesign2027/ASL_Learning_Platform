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
Give the glue components (authentication, notifications, configuration, logging) the same treatment. Revisit the D0 interfaces and revise any that grew too many parameters or leak internals.

 - do we have glue components?
   - authentication may be cool, but ultimately not necessary. e.g. having an account. i don't think we want to track stats like average accuracy and whatnot, so overall we probably don't need it initially. 
   - logging may be important for debugging, but generally there would be nothing to log. Potentially what the landmarks are, and potentially the outputs from the model (time, sign text, confidence, and sign accuracy), but i doubt those are truly necessary



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
