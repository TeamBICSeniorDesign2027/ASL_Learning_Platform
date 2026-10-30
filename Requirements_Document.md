# ASL Learning Platform — Requirements Document

## 1. Version and change log

| Version | Date | Change | Author |
|---|---|---|---|
| 1.0 | 2026-10-30 | First version of requirements document created from previous design documents. | Isaac Dowdy |

## 2. Purpose and scope

The purpose of this document is to provide an overview of the ASL Learning Platform's design requirements for the user, the functional and non-functional requirements, and the MoSCoW priorities important for the design process. It consolidates the user needs and acceptance criteria in [User_Stories.md](./User_Stories.md) with the architecture, interfaces, processing behavior, and traceability in the [high-level design folder](./docs/high_level_design/).

The current intended product is a browser-based practice tool for beginner and self-taught ASL learners. A learner uses a webcam to perform a supported sign, the system recognizes it, displays English text, and gives feedback about hand shape. Detailed design choices and detailed data formats for each component can be found in these design documents, particularly [Design D0](./docs/high_level_design/design_d0_document.md), [Design D1](./docs/high_level_design/design_d1_document.md), and [Design D2](./docs/high_level_design/design_d2_document.md).

### In scope

- Capturing data from the webcam on a web-based application
- Capturing a sign, recognizing supported vocabulary, and presenting an English text result.
- Showing confidence, sign-recognition status, and hand-shape feedback

### Out of scope for now

- Translating continuous ASL into complete English sentences.
- Recognition of every ASL sign or interpretation of all facial expressions.
- User accounts, learner profiles, progress history, or learning algorithms.

## 3. Stakeholders and user goals

| Stakeholder | Goal |
|---|---|
| Self-taught ASL learner (primary) | Practice independently and see the English text for a supported sign they performed. |
| Beginner ASL learner (primary) | Receive timely, understandable feedback about hand-shape accuracy so they can adjust their signing. |
| Deaf conversation partner (secondary) | Help a learner identify hand-shape errors that could make a sign harder to understand. |
| Privacy regulator / person whose image is processed | Have webcam imagery and extracted body/hand geometry handled only as needed for recognition and not retained as user data. |
| Project team and evaluator | Verify the specified behaviors using stable requirement IDs and documented test methods. |

These goals are pulled from [US-01 through US-04](./User_Stories.md)

## 4. Glossary

| Term | Definition |
|---|---|
| ASL | American Sign Language, a natural language with manual and non-manual features. |
| Sign | A performed ASL item or fingerspelled character |
| Fingerspelling | Using handshapes to represent individual letters, commonly used to spell names or words. |
| Learner | A person using the platform to practice signs. |
| Webcam frame | A still image sampled from the live webcam stream. |
| Landmark | A coordinate representing a tracked hand, face, or body point extracted from an image. |
| Sign window | The bounded sequence of frames associated with one signing attempt, including the point at which the learner finishes the sign. |
| Recognition confidence | The model's confidence value for a predicted class, represented on a 0-to-1 scale in the design. |
| Hand-shape feedback | A visual indication of how the learner's hand landmarks differ from a reference shape. |
| Supported vocabulary | The set of signs and fingerspelled characters our application is intended to recognize. The final set is an open decision. |
| Persistent storage | Data retained beyond the active processing/session lifecycle, including files, databases, logs, or analytics stores. |

## 5. Functional requirements

| ID | Requirement statement | Priority | Source | Verification method |
|---|---|---|---|---|
| REQ-01 | The system shall request the learner's permission before accessing the webcam and shall not start webcam capture when permission is denied. | Must | [US-01 / UC-01](./User_Stories.md); [D0](./docs/high_level_design/design_d0_document.md), I1–I2 | Deny webcam permission in a supported browser and verify that capture does not start and a clear message is shown. |
| REQ-02 | The system shall display the live webcam view during an active practice session and provide a control or equivalent action to end the session. | Must | [UC-01](./User_Stories.md); [D0](./docs/high_level_design/design_d0_document.md), Frame Capture and Send Feedback | Start a session, verify the live view is visible, end the session, and verify capture stops. |
| REQ-03 | For each signing attempt, the system shall extract the landmark sequence needed by the selected recognition model from the webcam frames. | Must | [UC-01](./User_Stories.md); [D0](./docs/high_level_design/design_d0_document.md), Hand Tracker; [D2](./docs/high_level_design/design_d2_document.md), Input Processing | Exercise a known test clip and verify that the processing pipeline produces a valid landmark sequence for the attempt. |
| REQ-04 | The system shall treat a signing attempt as complete when the learner's hands return to a resting position for at least 1 second, as defined in the current acceptance criteria. | Must | [AC-01.1](./User_Stories.md) | Perform a test sign followed by at least 1 second of rest; verify that the attempt is finalized only after the rest interval. |
| REQ-05 | The system shall classify a completed signing attempt | Must | [D0](./docs/high_level_design/design_d0_document.md), ML Model; [D1 research](./docs/high_level_design/d1_research.md) | Run tests and confirm that the model output maps to expected labels. |
| REQ-06 | When a supported sign is recognized, the system shall display its corresponding English text to the learner. | Must | [US-01 / AC-01.1](./User_Stories.md); [D2](./docs/high_level_design/design_d2_document.md), Results Processing and Send Feedback | For each approved acceptance sample, compare the displayed text with the expected English text; the displayed text must match exactly. |
| REQ-07 | The system shall not present a low-confidence classification as a recognized sign; it shall display a “Sign not recognized” message when confidence is below the approved recognition threshold. | Must | [UC-01 alternate flow](./User_Stories.md); [D2](./docs/high_level_design/design_d2_document.md), Results Processing | Supply outputs below and at the approved threshold; verify that below-threshold outputs are not shown as recognized and threshold-equal behavior follows the approved rule. |
| REQ-08 | When a sign is recognized and a reference shape is available, the system shall present hand-shape accuracy feedback to the learner. | Must | [US-02](./User_Stories.md); [D2](./docs/high_level_design/design_d2_document.md), Shape-comparison algorithm and Send Feedback | Use reference and deliberately varied hand-shape samples; verify that a score and the applicable visual corrections are displayed. |
| REQ-09 | If the learner's tracked hands remain outside the webcam frame for more than 1 second, the system shall pause translation output and display an out-of-frame warning until tracking is reacquired. | Must | [AC-01.2](./User_Stories.md); [UC-01 exception flow](./User_Stories.md) | Move hands out of view for less than and then more than 1 second; verify warning timing, paused output, and clearing after reacquisition. |
| REQ-10 | When webcam access, landmark extraction, model prediction, or result processing fails, the system shall display a user-understandable error or recovery instruction instead of presenting the failed attempt as a successful recognition. | Must | [D0](./docs/high_level_design/design_d0_document.md), interface error handling; [D2](./docs/high_level_design/design_d2_document.md), Results Processing | Inject each failure type and verify a visible, non-success error state and the appropriate recovery instruction. |
| REQ-11 | The system shall not save webcam frames or extracted learner landmarks to persistent storage as part of normal practice-session operation. | Must | [US-04](./User_Stories.md); [D0](./docs/high_level_design/design_d0_document.md), Frame Capture; [D1](./docs/high_level_design/design_d1_document.md), data model | Inspect browser storage, application files, server storage, and logs during and after a test session; verify no frames or learner landmarks are retained. The unresolved transmission boundary is tracked in OQ-01. |
| REQ-12 | The system shall support both left- and right-handed signing for every sign in the approved release vocabulary, unless a documented sign-specific limitation is approved before release. | Should | [D2 traceability table](./docs/high_level_design/design_d2_document.md), requirement 10; [D2](./docs/high_level_design/design_d2_document.md), two-handed-sign open question | Test approved signs using left- and right-handed samples and document any approved sign-specific exception. |

## 6. Non-functional requirements

| ID | Requirement statement | Priority | Source | Verification method |
|---|---|---|---|---|
| NFR-01 | The system shall display the recognized English text within 3 seconds after the user's hands return to the resting position for at least 1 second. | Must | [AC-01.1](./User_Stories.md); [D0](./docs/high_level_design/design_d0_document.md) | Measure from the end of the required 1-second rest interval to display of the result over the agreed acceptance test set; every accepted run must meet 3 seconds. |
| NFR-02 | The sign-processing pipeline shall target less than 100 milliseconds of processing latency for each inference cycle on the minimum supported device. | Should | [D0](./docs/high_level_design/design_d0_document.md), architecture pattern and justification | Benchmark inference cycles on the minimum supported device and report percentile and maximum latency. |
| NFR-03 | The system shall not retain webcam frames or extracted learner landmarks after they are no longer required for the active recognition attempt, and shall not store them persistently. | Must | [US-04](./User_Stories.md); [D0](./docs/high_level_design/design_d0_document.md); [D1](./docs/high_level_design/design_d1_document.md) | Inspect memory lifecycle, browser storage, network traffic, server storage, and logs during a session and after session termination. Whether landmarks may be transmitted for inference must be resolved by OQ-01. |
| NFR-04 | Every user-visible recognition, tracking, or processing failure defined in REQ-09 and REQ-10 shall produce an explicit message or warning; the system shall not silently discard the attempt while appearing successful. | Must | [D2](./docs/high_level_design/design_d2_document.md), Results Processing and Send Feedback; [Peer review](./docs/high_level_design/peer_review_feedback.md), inconsistent model outputs | Exercise the listed failure and warning cases and confirm each produces the specified visible state. |
| NFR-05 | The model shall return a prediction in less than 5 seconds as an upper bound on any single recognition; the 3-second display target in NFR-01 remains the acceptance target. | Must | [D2 traceability table](./docs/high_level_design/design_d2_document.md), requirement 1; [AC-01.1](./User_Stories.md) | Run every sample in the approved representative test set on the minimum supported device and record end-to-end time. Verify each result is under 5 seconds. |
| NFR-06 | A learner shall be able to start a practice session, perform one sign, and locate the resulting text, status, and available shape feedback without needing assistance. In other words, the interface shall be usable and easily learnable. | Should | [US-01 / US-02](./User_Stories.md); [D2](./docs/high_level_design/design_d2_document.md), Send Feedback | Conduct a usability test with representative beginner learners. |

## 7. MoSCoW summary and inherited constraints

### MoSCoW summary

| Priority | Requirements | Summary |
|---|---|---|
| Must | REQ-01–REQ-11, NFR-01, NFR-03–NFR-05 | Core webcam practice, supported-sign results, shape feedback, explicit failure behavior, privacy handling, and measurable performance/reliability. |
| Should | REQ-12, NFR-02, NFR-06 | Left- and right-handed signing coverage, per-cycle performance target, and usability validation. |
| Could | hand-shape feedback for two-handed signs, expanded vocabulary, showing the next-most-likely sign when confidence is low. | Optional enhancements, included only if time and the agreed release scope allow. None is committed |
| Won't | Lessons, curriculum, or learning algorithms (e.g., adaptive difficulty, spaced repetition, or personalized recommendations), full continuous ASL-to-English sentence translation, full-vocabulary ASL recognition or interpretation of facial expressions, persistent accounts, learner history, or recordings. | These items are outside the current practice-tool scope or conflict with the constraints below |

### Inherited constraints

| ID | Constraint | Source / effect |
|---|---|---|
| CON-01 | Economic: spending must be kept to a minimum because there is no outside funding. Prefer free/open-source software, tools, and data; use the cheapest option only when no free one exists. | [Connor's Constraints essay](./docs/individual_constraints_essays/individual_constraints_essay_connor_slutsky.md). The product is not intended for sale or monetary gain. |
| CON-02 | Economic/legal: open-source components must be credited, used with permission where required, and used in compliance with their licenses. | [Connor's Constraints essay](./docs/individual_constraints_essays/individual_constraints_essay_connor_slutsky.md). |
| CON-03 | Cultural: the team is not fluent in ASL, so supported signs and feedback must be reviewed by someone knowledgeable in ASL before release. The product focuses on ASL only. | [Connor's Constraints essay](./docs/individual_constraints_essays/individual_constraints_essay_connor_slutsky.md). Reduces mistranslation risk; see Won't list. |
| CON-04 | Security: webcam data must not be stored longer than necessary, and open-source dependencies must be kept at current, patched versions. | [Connor's Constraints essay](./docs/individual_constraints_essays/individual_constraints_essay_connor_slutsky.md). |
| CON-05 | Security: if a non-local model or external service is used, only the minimum necessary data may be sent to it, and none if avoidable. | [Connor's constraints essay](./docs/individual_constraints_essays/individual_constraints_essay_connor_slutsky.md); relates to OQ-01. |
| CON-06 | Security: recorded content should be stored and processed within the software, without sending data to external services, even if this limits the computer vision or storage methods available. | [Isaac's constraints essay](./docs/individual_constraints_essays/dowdy_contraints_essay.md). Stricter than CON-09; relates to OQ-01. |
| CON-07 | Ethics: if any user data is ever collected (e.g., to train a model), users must receive a clear explanation and an opt-in/opt-out choice before recording or storing begins. Collecting less data is preferred over improving the model at the expense of privacy. | [Isaac's constraints essay](./docs/individual_constraints_essays/dowdy_contraints_essay.md). Normal sessions still store nothing (CON-03). |
| CON-08 | Diversity and culture: the interface must give clear, usable, accessible feedback for learners of varied backgrounds and experience, and must not present the supported vocabulary as the full depth of ASL. | [Isaac's constraints essay](./docs/individual_constraints_essays/dowdy_contraints_essay.md). Relates to NFR-06 and the Won't list. |


## 8. Assumptions and open questions

| ID | Type | Item | Owner | Target date | Related requirements |
|---|---|---|---|---|---|
| OQ-01 | Open question | Where will landmark extraction, model inference, and Results Processing run? Our earliest designs describe it happening locally, but the newer designs discuss an API model | Team | 2026-11-14 | REQ-11, NFR-03, NFR-04 |
| OQ-02 | Open question | What is the release vocabulary: fingerspelling, isolated signs, or both; which exact labels are included; and is the target of 50 additional signs still intended? | Team | 2026-11-14 | REQ-05–REQ-08, REQ-12 |

