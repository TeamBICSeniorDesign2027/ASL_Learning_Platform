# Technical Specification

## Version and Change Log
| Version | Date | Change | Author |
|---|---|---|---|
| 1.0 | 2026-10-30 | First version of technical specification created from previous design documents. | Braden Monnin |

## Architecture Summary

The ASL tool utilizes a hybrid client-server and data pipeline architecture. The architecture is designed to prioritize real-time performance and biometric data privacy. The system operates via a React-based web frontend that access the user's webcam using the HTML5 MediaDevices API (Video Intake). To optimize the performance and ensure data security, the video feed is processed client-side by MediaPipe (Input Processing) to extract lightweight 3D coordinate arrays of the hands and face. The frontend is delivered as a static web application that is React-based.

Once the user holds a resting position for 1 second and frontend extracts the landmark tensors, it packages them into a JSON payload and transmits them via HTTP POST to the Python backend. The data flows into a TensorFlow machine learning model to classify the sign and assign a confidence score (ML Model). The result goes into the Results Processing component which maps the class label to English test, compares the performed hand shape against the reference shape, and reports the confidence of the sign. 

The resulting JSON payload is returned to the frontend's Send Feedback component The React UI consumes the payload to update the DOM without blocking the main thread, overlaying real-time text translations, a confidence meter, and directional correction arrows directly onto the live webcam feed.

## Interfaces and Data Formats

| Boundary | Input | Output/Format | Protocol | Error Behavior |
| -------- | ----- | ------------- | -------- | -------------- |
| Video Intake | Physical hand movements (User) | MediaStream object (Video frames) | HTML5 MediaDevices | Webcam blocked/disabled: Caught by the browser and surfaces a UI warning to the user.
| Input Processing | MediaStream (Webcam video frames) | JSON Payload (Tensors: sign_window_id, face, left, right) | Local JS function call | Hands not in frame: MediaPipe returns null arrays; frame drops silently to prevent flickering UI.
| ML Model | JSON Tensor Payload | JSON Prediction (class, confidence, frame_size, landmarks) | HTTP POST (/predict) | Malformed Data/NaN: Returns 400 Bad Request. Model Failure: Returns 500 Internal Error.
| Results Processing | JSON Prediction Payload | JSON FeedbackPayload (status, recognized_text, shape scores) | HTTP POST (/results) | Unknown Label / Low Confidence: Returns HTTP 200 but with payload status: error or not_recognized.
| Send Feedback | JSON FeedbackPayload | Rendered DOM Updates (React State) | React State Render | Missing/Stale ID: Payload is silently dropped to prevent crashing the active video feed.

### API Endpoint Table

| Method | Endpoint | Request Payload | Response Payload | Error Behavior and Codes |
| ------ | -------- | --------------- | ---------------- | -------------------------|
| POST | /api/v1/predict | JSON: { sign_window_id: int, face: float[][], left: float[][], right: float[][] } | 200 OK: JSON { class: string, confidence: float, frame_size: [int, int], ...landmarks } | 400 Bad Request: Missing tensors or invalid data. 500 Internal Error: Model execution failure (returns { error, message }). |
| POST | /api/v1/results | JSON: { sign_window_id: int, class: string, confidence: float, frame_size: [int, int], left: float[][], right: float[][] } | 200 OK: JSON FeedbackPayload containing status, recognized_text, overall_confidence, and shape | 400 Bad Request: Malformed JSON or missing sign_window_id. 200 OK (with status: error): Unknown label or low confidence below threshold|

## Technology Stack

| Layer | Technology Choice | Justification |
|-------| ----------------- | ------------- |
| Frontend | React JS | NFR-06 (Usability): Component state management allows instant rendering of feedback and error overlays without page reloads. |
| CV Pipeline | MediaPipe | CON-05 (Minimal Data): Extracts lightweight coordinates in the browser, preventing the need to stream raw video. |
| Backend | Python | REQ-05: Native environment for TensorFlow; highly efficient for the matrix math required by Results Processing. |
| ML Framework | TensorFlow | NFR-05 (Prediction Time): Optimized graph execution ensures inference completes well within the 5-second upper bound. |
| Deployment | Azure | CON-01 (Budget): Has a free tier that can be used to limit cost to the team. |

## Known Constraints

CON-01 (Minimum Spending): Forces the use of free-tier hosting (Azure) and open-source libraries. Rules out paid external APIs or heavy GPU cloud instances.

CON-02 (Open-Source Compliance): Forces the inclusion of an attribution/credits section in the UI for React, MediaPipe, and TensorFlow.

CON-03 (ASL Fluency/Review): The supported signs must come from a reputable dataset and feedback must be reviewed by someone knowledgeable in ASL before release.

CON-04 & CON-07 (Security & Ethics): Normal sessions rule out the creation of data-collection endpoints. No telemetry or training data can be gathered from users. Must be opt-in and clear explanation if were to collect data.

CON-05 & CON-06 (Data Transmission): Forces the architecture to use client-side MediaPipe to extract landmarks. Rules out sending raw video files to the backend server.

CON-08 (Accessibility): Forces the UI to provide visual (directional arrows) and text-based feedback rather than relying solely on color indicators. Must not present the supported vocabulary as the full depth of ASL.

## Traceability Matrix

| Req ID | Description | Design Component (D0/D1/D2) | Status | Test Case |
| ------ | ----------- | --------------------------- | ------ | --------- |
| REQ-01 | Webcam permission check | D0: Frame Capture / Video Intake | Designed | TC-001  |
| REQ-02 | Display live view & stop session | D0: Frame Capture / Send Feedback | Designed | TC-002 |
| REQ-03 | Extract landmark sequence | D2: Input Processing (MediaPipe) | Designed | TC-003 |
| REQ-04 | 1-second rest to complete attempt | D2: Input Processing | Planned | TC-004 |
| REQ-05 | Classify completed attempt |	D0/D1: ML Model | Designed | TC-005 |
| REQ-06 | Display English text | D2: Results Processing / Send Feedback | Designed | TC-006 |
| REQ-07 | Low-confidence warning message | D2: Results Processing (Confidence Check) | Designed | TC-007 |
| REQ-08 | Hand-shape accuracy feedback | D2: Results Processing (Shape Scoring) | Designed | TC-008 | 
| REQ-09 | Out-of-frame warning (> 1s) | D2: Input Processing | Designed | TC-009 | 
| REQ-10 | Explicit error handling | D2: All APIs / Send Feedback | Designed | TC-010 | 
| REQ-11 | No persistent storage used | D1: Data Model (None) | Designed | TC-011 | 
| REQ-12 | Left/Right hand support | D1: ML Model Training	| Designed | TC-012 | 
| NFR-01 | Text displayed within 3s | D1/D2: ML Model + UI pipeline	 | Planned | TC-013 | 
| NFR-02 | <100ms inference latency | D1/D2: ML Model + UI pipeline | Planned | TC-014 |
| NFR-03 | Memory flushed after session | D1: Data Model / React unmount | Designed | TC-015 |
| NFR-04 | Errors are visible, not silent | D2: Send Feedback | Designed | TC-016 |
| NFR-05 | Prediction bound < 5s | D1: ML Model	 | Planned | TC-017 |
| NFR-06 | Usability / Learnability | D2: Send Feedback (UI Layout) | Planned | TC-018 |
| CON-01 | Free/OSS solutions | D1: Tech Stack | Designed | TC-019 |
| CON-02 | License compliance | D2: Send Feedback (About Page) | Planned | TC-020 |
| CON-03 | ASL review for vocabulary | D1: ML Model Classes | Planned | TC-021 |
| CON-04 | Keep dependencies patched | CI/CD Pipeline (GitHub Dependabot) | Planned | TC-022 |
| CON-05 | Minimum data sent to backend | D2: Input Processing (Landmarks only) | Designed | TC-023 |
| CON-06 | No external data services for recorded content | D2: Input Processing done locally | Designed | TC-024 |
| CON-07 | Opt-in explanation if collected | D1: Data Model (Data not collected) | Designed | TC-025 |
| CON-08 | Accessible/diverse UI | D2: Send Feedback | Designed | TC-026 |