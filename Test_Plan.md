# Current Test Cycle
What is being tested this cycle, by requirement ID and component, and what is not. The out-of-scope list names deferred Won't and Could features, known limitations, and third-party components you will not retest.

**In Scope:**
|Requirement ID|Requirement|Component|
|--------------|-----------|---------|
| REQ-01 | The system shall request the learner's permission before accessing the webcam and shall not start webcam capture when permission is denied. |Video Intake|
| REQ-02 | The system shall display the live webcam view during an active practice session and provide a control or equivalent action to end the session. |Video Intake|
| REQ-03 | For each signing attempt, the system shall extract the landmark sequence needed by the selected recognition model from the webcam frames. | Input Processing|
| REQ-04 | The system shall treat a signing attempt as complete when the learner's hands return to a resting position for at least 1 second, as defined in the current acceptance criteria. | Input Processing|
| REQ-05 | The system shall classify a completed signing attempt | ML Model |
| REQ-06 | When a supported sign is recognized, the system shall display its corresponding English text to the learner. | Send Feedback|
| REQ-07 | The system shall not present a low-confidence classification as a recognized sign; it shall display a "Sign not recognized" message when confidence is below the approved recognition threshold. | Results Processing / Send Feedback |
| REQ-08 | When a sign is recognized and a reference shape is available, the system shall present hand-shape accuracy feedback to the learner. | Results Processing / Send Feedback | 
| REQ-09 | If the learner's tracked hands remain outside the webcam frame for more than 1 second, the system shall pause translation output and display an out-of-frame warning until tracking is reacquired. |Video Intake / Send Feedback |
| REQ-10 | When webcam access, landmark extraction, model prediction, or result processing fails, the system shall display a user-understandable error or recovery instruction instead of presenting the failed attempt as a successful recognition. | Send Feedback |
| REQ-11 | The system shall not save webcam frames or extracted learner landmarks to persistent storage as part of normal practice-session operation. | Input Processing / Video Intake| 
| NFR-01 | The system shall display the recognized English text within 5 seconds after the user's hands return to the resting position for at least 1 second. | ML Model / Results Processing | 
| NFR-03 | The system shall not retain webcam frames or extracted learner landmarks after they are no longer required for the active recognition attempt, and shall not store them persistently. | Input Processing / Video Intake |
| NFR-04 | Every user-visible recognition, tracking, or processing failure defined in REQ-09 and REQ-10 shall produce an explicit message or warning; the system shall not silently discard the attempt while appearing successful. | Send Feedback |
| NFR-05 | The model shall return a prediction in less than 5 seconds as an upper bound on any single recognition; the 3-second display target in NFR-01 remains the acceptance target. |ML Model / Results Processing |    


**Out of Scope:**
|Requirement ID|Requirement|Component|Reason For Exclusion|
|--------------|-----------|---------|--------------------|
| REQ-12 | The system shall support both left- and right-handed signing for every sign in the approved release vocabulary, unless a documented sign-specific limitation is approved before release. | Input Processing / ML Model | Dependent on if we can find the data for left-handed signs. Not a strict requirement for the application to be considered successful and should only be pursued if time allows| 
| NFR-02 | The sign-processing pipeline shall target less than 100 milliseconds of processing latency for each inference cycle on the minimum supported device. |ML Model|This is a strict latency requirement who's feasible is still unknown to us, certainly an eventual goal, but not during this first cycle|
| NFR-06 | A learner shall be able to start a practice session, perform one sign, and locate the resulting text, status, and available shape feedback without needing assistance. In other words, the interface shall be usable and easily learnable. |Video Intake|Compared to the other requirements, this requirement is lower importance and not fully necessary for the project to be considered a success.|


# Verification and validation strategy. 
Half a page saying which requirements are verified against the spec by the team and which are validated against stakeholder needs by users or your advisor, plus a test-pyramid table of layer, approximate count, and coverage focus. Hardware, sensor, and machine learning projects also state variables, metrics, tolerance, and trials.

**Breakdown of Who Verifies**

|Requirement(s)|Verification/Validation|Performed by|
|--------------|----|------------|
| REQ-01, REQ-02, REQ-09 | Verification | Team |
| REQ-03, REQ-04 | Verification | Braden |
| REQ-05, REQ-06, REQ-07 | Verification | Connor |
| REQ-08 | Verification and validation | Braden (verification); ASL-knowledgeable reviewer, advisor (validation) |
| REQ-10, NFR-04 | Verification | Isaac |
| REQ-11, NFR-03 | Verification | Team | 
| NFR-01, NFR-05 | Verification | Team | 
| Whole product (US-01, US-02) | Validation | Beginner and self-taught ASL learners, advisor |

**Test Pyramid**

|Layer|Approx. Count|Coverage Focus|
|-----|-------------|--------------|
| Unit | 30 | Important functions and modules within individual components (Input Processing, ML Model, Results Processing, Feedback)|
| Integration | 10 | Testing the interactions and flow between components, capturing errors and edge-cases effectively as the data moves through the pipeline |
| System | 5 | Every in-scope requirement in a real browser with a real webcam; testing the whole system together. |
| Acceptance | 3 | Testing that the system meets the requirements with live demos with a beginner ASL learner, a seasoned ASL learner, and our advisor |

**ML Metrics**

The ML Model (REQ-05 to REQ-07) and shape scoring (REQ-08) are the parts whose results depend on data, so they are checked statistically rather than with a single run.

|Item|Definition|
|----|----------|
| Independent variables | Sign class; signer; lighting; camera distance; background |
| Controlled variables | `CONFIDENCE_THRESHOLD` (0.80) and `DEVIATION_TOLERANCE` (0.25) |
| Metrics | Top-1 accuracy overall; confusion matrix to find sign pairs that are mixed up; false-accept rate (an incorrect sign shown as recognized at confidence of 0.80 or higher); rejection rate for non-sign input; shape score for a quality attempt versus a deliberately wrong shape |
| Tolerance | Overall top-1 accuracy of at least 90% on a sample of the vocabulary. False-accept rate of 5% or less. A reference-quality attempt scores 0.80 or more and a deliberately wrong shape scores 0.50 or less. |
| Trials | At least 20 attempts per sign in the release vocabulary, from at least 3 signers, across at least 2 lighting conditions. Non-sign input is tested with 20 or more trials as well. |


# Success Criteria
A table of measurable success criteria: metric, target threshold, how it is measured, and the Week 9 requirement ID behind it. Cover performance, reliability, usability, and accuracy as they apply.

|Metric|Target Threshold|How Measured|Requirement ID|
|------|----------------|------------|--------------|
| **Performance** | | | |
| Result display time | 3 s or less from the end of the 1 s rest to the text being displayed, on every run | Console timestamps over the full acceptance set | NFR-01 |
| Attempt completion timing | Attempt is finalized only after at least 1 s of rest | Test with rests of 0.5 s, 1 s and 1.5 s | REQ-04 |
| Out-of-frame warning timing | No warning at 1 s or less out of frame; warning and paused output after more than 1 s; warning clears once tracking returns | Tests with hands out for 0.5 s, 1.5 s and 3 s | REQ-09 |
| **Reliability** | | | |
| Errors and Warnings | all defined error and warning cases display an understandable message | Test each case and check for messages | REQ-10, NFR-04 |
| Webcam permission denied | 0 frames captured and a message shown| Deny permission in each supported browser | REQ-01 |
| Retained learner data | No frames or landmark arrays in browser storage, memory after the attempt, server storage or logs | Storage, memory and log inspection after successful and failed sessions | REQ-11, NFR-03 |
| **Accuracy** | | | |
| Recognition accuracy | At least 90% top-1 accuracy | 20 or more attempts per sign in the chosen sample | REQ-05 |
| Displayed text correctness | Text displayed to the user matches the translation of the recognized sign, every time. | Compare the displayed string with the expected label | REQ-06 |
| Low-confidence rejection | Confidence below 0.80 always shows "Sign not recognized"; 0.80 exactly counts as recognized; false-accept rate of 5% or less | Non-sign input trials, and console inspection to see confidence values compared with displayed feedback | REQ-07 |
| Shape feedback | Quality attempt scores 0.80 or more; deliberately wrong shape scores 0.50 or less; arrows point toward the template (dx and dy sign correct) | varied hand-shape samples | REQ-08 |
| **Usability** | | | |
| Learner usability | Pilot learners report that the learning experience is intuitive and helpful | Post-session survey | US-01, US-02 |
| Browser support | All tests pass in each of the latest versions of Chrome, Firefox, Edge and Safari | Run the system tests in each browser | REQ-01, REQ-02 |


# Test cases
At least 3 cases for every major requirement, and every Must requirement is major. Use the five-column template below, and include at least one edge case or failure condition in each group. Number the IDs to match the Test Case column of your Week 9 traceability matrix.

|ID|Req ID|Description|Steps|Expected Result|Status|
|--|------|-----------|-----|---------------|------|
|TC-1| REQ-01 |Webcam Permission Accepted|1) Open a supported browser, 2) Navigate to the app, 3) Receive prompt automatically from browser to allow webcam, 4) Click 'Allow'|Webcam footage should now be recorded - the user should see their webcam footage displayed on the browser|Not Run|
|TC-2| REQ-01 |Webcam Permission Denied|1) Open a supported browser, 2) Navigate to the app, 3) Receive prompt automatically from browser to allow webcam, 4) Click 'Deny'|Webcam footage should now be not recorded - the user should see an error message that the app can't receive webcam footage|Not Run|
|TC-3| REQ-01 |Webcam Permission Unsupported Browser|1) Open an unsupported browser (e.g. Internet Explorer 11), 2) Navigate to the app |No prompt should be received by the browser to access webcam - the user should see an error message that the app can't receive webcam footage|Not Run|
|TC-4| REQ-02 |Practice Session Started|1) Open a supported browser, 2) Navigate to the app, 3) Allow webcam, 4) Click Start Session |The user should see their webcam footage being recorded on the UI, there should be an indicator that a session is currently recording|Not Run|
|TC-5| REQ-02 |Practice Session Stopped|1) Open a supported browser, 2) Navigate to the app, 3) Allow webcam, 4) Click Start Session, 5) Click End Session|The user should see their webcam footage is still being shown, but that there should be no indicator that the footage is being recorded. On clicking the button, there should also be an indicator that the session has stopped|Not Run|
|TC-6| REQ-02 |Practice Session Unsupported Browser|1) Open an unsupported browser (e.g. Internet Explorer 11), 2) Navigate to the app|The user should see an error that webcam footage cannot be recorded, and the Start Session button should be grayed out|Not Run|
|TC-7| REQ-03 |Extract Landmarks Valid|1) Start a session, 2) make a sign for 'A' |Valid landmarks should be returned (i.e. print to the console). Example MediaPipe output looks something like {"faceLandmarks":      [ [ {"x":…, "y":…, "z":…}, /* 478 \*/ ] ],"poseLandmarks":      [ [ {"x":…, "y":…, "z":…, "visibility":…}, /* 33 \*/ ] ],"poseWorldLandmarks": [ [ /* 33 \*/ ] ],"leftHandLandmarks":  [ [ /* 21 \*/ ] ],"rightHandLandmarks": [ [ /* 21 */ ] ]}|Not Run|
|TC-8| REQ-03 |Extract Landmarks Invalid|1) Start a session, 2) Point the webcam at a random object in the room and wait for a response | A null object or object with many null values should be returned|Not Run|
|TC-9| REQ-03 |Extract Landmarks Partial|1) Start a session, 2) Begin to perform the sign for 'A' but let your hands leave the frame partway through|An object with a valid structure should return, but there should be null values corresponding with the obscured parts |Not Run|
|TC-10| REQ-04 |Attempt Completes After Rest|1) Start a session, 2) Make the sign for 'A', 3) Lower both hands to a resting position and hold still for at least 1 second (e.g. 1.5 seconds)|The attempt is not finalized while the rest interval is under 1 second. It is finalized once the rest has lasted at least 1 second, and a completion event or log (e.g. "attempt complete" with a timestamp) appears in the console. The captured landmark sequence for the attempt contains the sign and ends where the rest begins.|Not Run|
|TC-11| REQ-04 |Rest Interval Too Short (Boundary)|1) Start a session, 2) Make the sign for 'A', 3) Lower both hands to a resting position for about 0.5 seconds, 4) Raise your hands again and continue signing|The attempt is not finalized after the 0.5 second rest, and no completion event or prediction is triggered. The rest timer resets when the hands leave the resting position, and the new movement begins a new attempt.|Not Run|
|TC-12| REQ-04 |Hands Never Return to Rest (Failure)|1) Start a session, 2) Make the sign for 'A', 3) Keep your hands raised in the frame, holding the sign or moving continuously, for over 30 seconds without resting|The attempt is not finalized, and no prediction is produced during those 30 seconds. The app does not crash, freeze, or finalize early. After the 30 seconds, regardless if the user is finishing signing, a prediction is attempted (e.g. landmarks retrieved, sent to ML Model) for the first 30 seconds of signing. 30 seconds should be the maximum duration for a session.|Not Run|
|TC-13| REQ-05 |Classify Known Sign Valid|1) Start a session, 2) Make the sign for 'A' clearly and rest for at least 1 second to complete the attempt|The completed attempt is passed to the model and a classification result is displayed to the user. The label is one of the model's supported labels, and it matches the expected label for the sign performed.|Not Run|
|TC-14| REQ-05 |Classify Multiple Signs Mapping|1) Start a session, 2) Make the sign for 'A' 3) Immediately after, make the sign for 'B' and rest for 1 second, 4) Immediately after, make the sign for 'C' and rest for 1 second|Each attempt is classified separately, and the predicted labels are "A", "B", and "C" in that order. No attempt is skipped, merged, or classified using landmarks from a previous attempt.|Not Run|
|TC-15| REQ-05 |Classify Unsupported or Ambiguous Input (Failure)|1) Start a session, 2) Make a gesture that is not a supported sign (e.g. a wave or a random hand shape), 3) Rest for at least 1 second to complete the attempt|The app does not crash. The model returns an "unrecognized" outcome.|Not Run|
|TC-16| REQ-06 |Display Text Exact Match|1) Start a session, 2) Make the sign for 'A' and rest for at least 1 second to complete the attempt|The English "A" is displayed to the learner. The displayed text matches the expected text for the sign exactly, including case, spelling, and punctuation, with no extra characters or whitespace.|Not Run|
|TC-17| REQ-06 |Display Updates Between Attempts|1) Start a session, 2) Make the sign for 'A' and rest for 1 second, 3) Note the displayed text, 4) Make the sign for 'B'' and rest for 1 second|After the first attempt the display shows "A". After the second attempt it shows "B", replacing the earlier text (or clearly showing the latest result, depending on the design). The text from the first attempt does not persist incorrectly.|Not Run|
|TC-18| REQ-06 |No Text for Unrecognized Sign (Failure)|1) Start a session, 2) Make a gesture that is not a supported sign, 3) Rest for at least 1 second to complete the attempt|No English text for a supported sign is displayed. The app either shows a clear message (e.g. "Sign not recognized"), and it does not display a wrong or stale label from an earlier attempt.|Not Run|
|TC-19| REQ-07 |Below Threshold Not Recognized|1) Start a session, 2) Supply deliberately sloppy sign for 'A', 3) Complete the attempt with at least 1 second of rest|The message "Sign not recognized" is displayed. The English text for the predicted sign (e.g. "A") is not displayed, and no hand-shape accuracy feedback is shown as if the sign were recognized.|Not Run|
|TC-20| REQ-07 |Confidence Exactly At Threshold (Boundary)|1) Supply clip with a classification output with confidence exactly equal to the approved threshold|The behavior follows the approved rule for threshold-equal values. The sign text is displayed. The result is the same on repeated runs.|Not Run|
|TC-21| REQ-07 |Above Threshold Recognized|1) Start a session, 2) Perform a neat and clear sign for 'A', 3) Complete the attempt with at least 1 second of rest|The sign is treated as recognized. Its English text is displayed and "Sign not recognized" is not shown.|Not Run|
|TC-22| REQ-08 |Feedback With Reference Shape|1) Start a session, 2) Make the sign for 'A' neatly and clearly, 3) Complete the attempt with at least 1 second of rest|The sign is recognized and a hand-shape accuracy score is displayed. The score is high for a close match, and the feedback is shown alongside the recognized text.|Not Run|
|TC-23| REQ-08 |Deliberately Varied Hand Shape|1) Start a session, 2) Make the sign for 'A' with a deliberate error that is still recognized (e.g. thumb tucked in the wrong place, or fingers not fully curled), 3) Complete the attempt with at least 1 second of rest|The sign is still recognized, and the displayed score is lower than in TC-22. Visual corrections applicable to the error are shown (e.g. an overlay or highlight on the thumb or the incorrect fingers). Corrections are not shown for parts of the hand that match the reference.|Not Run|
|TC-24| REQ-08 |No Reference Shape Available (Failure)|1) temporarily remove the reference for 'A', 2) Start a session, 3) Make a sign for 'A', 4) Complete the attempt with at least 1 second of rest|The recognized sign's English text is still displayed. No score or visual corrections are shown, and the app does not crash or display a score of 0, NaN, or undefined. A clear message that feedback is unavailable is displayed.|Not Run|
|TC-25| REQ-09 |Brief Out-of-Frame (Under 1 Second)|1) Start a session, 2) Begin the sign for 'A' with your hands in frame, 3) Move both hands out of view for about 0.5 seconds, 4) Move your hands back into frame and finish the sign|No out-of-frame warning is displayed, and translation output is not paused. The attempt continues as normal, and the hands are tracked again without a visible interruption.|Not Run|
|TC-26| REQ-09 |Out-of-Frame Over 1 Second, Then Reacquired|1) Start a session, 2) Make the sign for 'A' with your hands in frame, 3) Move both hands out of view for about 2 seconds, 4) Observe the screen, 5) Move your hands back into frame|After more than 1 second out of frame, an out-of-frame warning is displayed and translation output is paused (no new recognized text or feedback appears while the warning is up). When the hands are reacquired, the warning clears and translation output resumes. Console timestamps show the warning appeared after the 1 second threshold, not before.|Not Run|
|TC-27| REQ-09 |Hands Stay Out of Frame Indefinitely (Failure)|1) Start a session, 2) Move both hands out of view and keep them out for at least 30 seconds, 3) Observe the screen, 4) Move your hands back into frame|The warning stays visible the whole time, and no translation output, sign text, or "Sign not recognized" message is produced while it is shown. The app does not crash, freeze, or finalize an empty attempt. Once the hands return, tracking resumes and the warning clears.|Not Run|
|TC-28| REQ-10 |Webcam Access Failure|1) Start a session, 2) Cause a webcam failure (e.g. unplug the webcam, have another application take exclusive control of it mid-session, disable your webcam), 3) Observe the screen|A visible error state is displayed (e.g. "Unable to Access Webcam"). No recognized sign, sign text, or accuracy score is displayed, and the failed state is not shown as a success.|Not Run|
|TC-29| REQ-10 |Landmark Extraction Failure|1) Start a session, 2) Make the sign for 'A' or a moving sign, but go out of frame for a short time before coming to rest|A visible, non-success error state is displayed with a recovery instruction (e.g. "We couldn't track your hands. Try again or reload the page"). The attempt is not classified or shown as recognized, and no unhandled exception appears to the learner. Verify the outputted landmarks from MediaPipe contain partial None/NaN/undefined values|Not Run|
|TC-30| REQ-10 |Model Prediction Failure|1) Disconnect the ML Model from the UI (e.g. stop the backend from running) 2) Start a session, 3) Make the sign for 'A'|A visible error state is displayed with a recovery instruction (e.g. "Cannot access model. Please try again another time"). No predicted label, English text, or accuracy score appears, and the attempt is not presented as a successful recognition. "Sign not recognized" is also not used for this case, since it is a system failure and not a low-confidence result.|Not Run|
|TC-31| REQ-10 |Result Processing Failure|1) Start a session, 2) Inject a failure in result processing (e.g. a stubbed processing step that throws an error after a valid prediction is returned), 3) Make the sign for 'A' and rest for 1 second|A visible error state is displayed with a recovery instruction, and the valid prediction is not shown as a successful result. After the error, the learner can start a new attempt (via the recovery instruction) without reloading the whole app, if the design allows it.|Not Run|
|TC-32| REQ-11 |No Browser Storage After Session|1) Open the browser developer tools, 2) Note the contents of localStorage, sessionStorage, IndexedDB, Cache Storage, and cookies, 3) Start a session and complete several attempts (e.g. 'A', 'B', 'C'), 4) End the session and inspect the same storage areas again|No webcam frames (images, blobs, or data URLs) and no landmark data (arrays of x/y/z values) are present in any browser storage area. Any entries that exist (e.g. app settings) contain no frames or learner landmarks and were already present before the session or hold only non-learner data.|Not Run|
|TC-33| REQ-11 |No Files or Logs Retained|1) Start a session and complete several attempts, 2) End the session, 3) Inspect application files, server storage (if any), and server or application logs created or modified during the session, 4) Search them for image files, video files, and landmark values|No frame images, video recordings, or landmark data are found in application files, server storage, or logs. Logs may record events (e.g. "attempt completed") but contain no frames or landmark coordinates.|Not Run|
|TC-34| REQ-11 |No Retention After Abnormal End (Failure)|1) Start a session and begin an attempt, 2) End the session abnormally partway through (e.g. close the tab, refresh the page, or trigger a failure from TC-28), 3) Reopen the app and inspect browser storage, application files, server storage, and logs as in TC-32 and TC-33|No partial frames or landmark data were left behind by the interrupted session, and the new session does not contain data from the old one.|Not Run|
|TC-35| NFR-01 |Display Within 5 Seconds|1) Start a session, 2) Make the sign for 'A', 3) Rest your hands for at least 1 second, 4) Using console timestamps, record the 5time the 1-second rest interval ended and the time the English text was displayed|The English text is displayed no more than 5 seconds after the end of the 1-second rest interval. The measured elapsed time is recorded in the test results.|Not Run|
|TC-36| NFR-01 |Full Acceptance Set, Every Run|1) Start a session, 2) Run every sample in the agreed acceptance test set (live or from recorded clips), 3) For each sample, record the elapsed time from the end of the 1-second rest to the display of the result|Every accepted run meets the 5 second limit. The result is judged on the slowest run, not the average. Any run over 5 seconds is logged as a failure of NFR-01, with the sample and its time recorded.|Not Run|
|TC-37| NFR-01 |First Attempt After Page Load (Edge)|1) Fully reload the page with the cache cleared, 2) Start a session, 3) Immediately make the sign for 'A' as the very first attempt, 4) Rest for 1 second and record the elapsed time to display|The first attempt, which includes model loading and warm-up, still displays the text within 5 seconds. If it doesn't, record the overshoot and whether later attempts meet the limit, so the problem can be traced to cold-start cost.|Not Run|
|TC-38| NFR-03 |Data Released After Attempt|1) Open the browser developer tools (Memory and Application tabs), 2) Start a session and complete one attempt, 3) After the result is displayed, take a memory snapshot and search it for frame buffers and landmark arrays from that attempt|Frames and landmarks from the completed attempt are no longer held in memory beyond what is needed for the active attempt (e.g. they are cleared, overwritten, or eligible for garbage collection). No frame or landmark data from the completed attempt is found in browser storage.|Not Run|
|TC-39| NFR-03 |No Retention After Session Termination|1) Start a session and complete several attempts, 2) End the session, 3) Inspect memory, browser storage, server storage, and logs, 4) Watch the Network tab during the session for outgoing requests|No frames or learner landmarks remain in memory, browser storage, server storage, or logs after the session ends. Network traffic is recorded for review, and any frame or landmark payloads are flagged and reported against OQ-01.|Not Run|
|TC-40| NFR-03 |Data Released After Failed Attempt (Failure)|1) Start a session, 2) Begin an attempt and cause a failure partway through (e.g. the hands leave the frame for over 1 second, or inject a prediction failure from TC-30), 3) After the error or warning appears, inspect memory, browser storage, and logs|The incomplete attempt's frames and landmarks are discarded when the failure occurs, and they are not kept for retry or debugging. Nothing is written to persistent storage by the failure handling code.|Not Run|
|TC-41| NFR-04 |Every Defined Failure Shows a Message|1) Start a session, 2) One at a time, trigger each failure and warning from REQ-09 and REQ-10 (hands out of frame over 1 second, webcam failure, landmark extraction failure, model prediction failure, result processing failure), 3) Record what the learner sees for each|Each of the five cases produces an explicit, visible message or warning that matches the state specified for it in REQ-09 or REQ-10. None of them ends with a blank screen, a frozen screen, or the old result still showing.|Not Run|
|TC-42| NFR-04 |Failed Attempt Not Shown as Success|1) Start a session, 2) Trigger a failure during an attempt (e.g. inject a model prediction failure), 3) Check the results display, any attempt history or session summary, and the console|The failed attempt does not appear as a recognized sign, a correct result, or a completed attempt with a score, in the main display, in any history or summary, or in the console. It may appear only as a clearly labelled failure.|Not Run|
|TC-43| NFR-04 |Silent Discard Check (Failure)|1) Start a session, 2) Trigger each failure type from REQ-09 and REQ-10 while watching the screen and the console, 3) For each, confirm that the attempt was not just dropped, so that the app returns to its normal state with no explanation|In no case does the app return to a normal-looking state with no message after an attempt fails. If a failure produces no visible message, it is logged as a defect. After each failure the learner can continue (e.g. start a new attempt) with the app in a consistent state.|Not Run|
|TC-44| NFR-05 |Prediction Under 5 Seconds|1) On the minimum supported device (e.g. smartphone), start a session, 2) Make the sign for 'A' and rest for 1 second, 3) Record the time from the completed attempt being handed to the model to the prediction being returned|The prediction is returned in under 5 seconds. The measured time is recorded along with the device used.|Not Run|
|TC-45| NFR-05 |Full Representative Set on Minimum Device|1) On the minimum supported device, run every sample in the approved representative test set, 2) For each sample, record the end-to-end time from the start of recognition to the result being returned|Every result is returned in under 5 seconds, judged on the slowest run. The results are also compared against the 5 second display target from NFR-01 as a secondary check, since the 5 second target remains the acceptance target.|Not Run|
|TC-46| NFR-05 |Under Load or Throttled CPU (Stress)|1) On the minimum supported device (or with 4x CPU throttling enabled in the browser developer tools), start a session, 2) Run several attempts back to back with no pause beyond the 1-second rest, 3) Record the time for each prediction|No single prediction takes 5 seconds or more, including later attempts in the run. If a prediction exceeds 5 seconds, the app shows a timeout or error message (per NFR-04) instead of hanging silently, and the overshoot is logged as a failure of NFR-05.|Not Run|


## Test Case Appendix 

**Supported Browsers**
 - latest two versions of Chrome, Firefox, Edge, and Safari on desktop
   - These browsers get updated constantly (~ every two weeks), so we shouldn't specify specific version numbers







# Test environment
The hardware, software, data, and configuration the tests run against: operating system and browser versions, devices and sensors, seeded test data, network conditions. Distinguish development, staging, and pilot environments.

**Architecture under test**: a JS React frontend, with MediaPipe extracting landmarks in the browser, sending JSON to a Python backend that runs a TensorFlow model and Results Processing.

- **Operating System**: Windows 11, latest macOS
- **Browsers**: latest versions of Chrome, Firefox, Edge, and Safari
- **Devices**: anu device with a built-in webcam
- **Tech Stack Versions**: versions of MediaPipe, python, TensorFlow, JavaScript, and React will be later recorded in the repository files as we begin implementation
- **Data**: A subset of ASL signs and the ASL alphabet, determined later
- **Network**: Campus Network, and the team's home network
- **Learning Environment**: any fixed conditions of bright or dim lighting with a plain background



# Pass and fail criteria
The objective threshold that decides whether each test, and the plan as a whole, succeeded. State the pass condition and what counts as a failure, tied to the metrics in part 3.

## Test-case

| Status | Condition |
|--------|-----------|
| **Pass** | Every statement in the expected result is true, every measured value is inside its threshold, and no fail condition occurred. |
| **Fail** | Any statement in the expected result is not met, or any fail condition occurred. |
| **Blocked** | The case could not be run because a precondition was missing. A blocked case counts as not passed and must be unblocked and run. |
| **Not Run** | The case has not been attempted yet. |

**Any one of these makes a case Fail:**
- The app crashes, freezes, or shows an unhandled exception to the learner.
- A failed or unrecognized attempt is presented as a successful recognition (REQ-10, NFR-04).
- A stale result from an earlier attempt is shown as the current result.
- Any webcam frame or learner landmark data is found in persistent storage (REQ-11, NFR-03).
- A measured time exceeds its limit on any single run.

## Metric level

| Metric | Pass | Fail | Req ID | Test cases |
|---------------------------|------|------|--------|------------|
| Webcam permission denied | 0 frames captured and an error message is shown | Any frame captured after denial, or no message shown | REQ-01 | TC-1 to TC-3 |
| Attempt completion timing | Attempt finalizes only after a rest of 1.0 s or more; a 0.5 s rest triggers nothing; a session caps at 30 s | Attempt finalizes after a rest under 1 s, never finalizes, or exceeds the 30 s cap | REQ-04 | TC-10 to TC-12 |
| Recognition accuracy | Top-1 accuracy of 90% or more over 20+ attempts per sign, 3+ signers, 2+ lighting conditions | Accuracy below 90% | REQ-05 | TC-13 to TC-15 |
| Displayed text correctness | Displayed string matches the expected label in 100% of recognized attempts | Any mismatch, or a stale label shown | REQ-06 | TC-16 to TC-18 |
| Low-confidence rejection | Confidence below 0.80 always shows "Sign not recognized" | Any below-threshold result shown as recognized or 0.80 exactly handled inconsistently | REQ-07 | TC-19 to TC-21 |
| Shape feedback | Reference-quality attempt scores 0.80 or more; deliberately wrong shape scores 0.50 or less; arrow direction correct in 100% of samples; no undefined score ever shown | Any score outside these bounds, wrong arrow direction, or an invalid score displayed | REQ-08 | TC-22 to TC-24 |
| Out-of-frame warning timing | No warning at 1.0 s or less out of frame; warning and paused output appear after more than 1.0 s (no later than 1.5 s); warning clears when tracking returns | Warning shown at 1.0 s or less, shown later than 1.5 s, output not paused, or warning does not clear | REQ-09 | TC-25 to TC-27 |
| Errors and warnings | 5 of 5 defined failure cases (out of frame over 1 s, webcam, landmark extraction, model prediction, result processing) show an explicit, understandable message; 0 silent discards | Any failure case with no message, a blank or frozen screen, or a failure shown as success | REQ-10, NFR-04 | TC-28 to TC-31, TC-41 to TC-43 |
| Retained learner data | 0 frames or landmark arrays found in browser storage, application files, server storage, or logs after normal, failed, and abnormally ended sessions; logs may contain events only | 1 or more frames or landmark arrays found in any location | REQ-11, NFR-03 | TC-32 to TC-34, TC-38 to TC-40 |
| Result display time | English text displayed within 5 s of the end of the 1 s rest, on every run of the acceptance set, including the first attempt after page load | Any single run over 5 s | NFR-01 | TC-35 to TC-37 |
| Prediction time | Prediction returned in under 5 s on every run, on the minimum supported device and under 4x CPU throttling; a timeout shows an error message instead of hanging | Any prediction at 5 s or more, or a silent hang | NFR-05 | TC-44 to TC-46 |
| Browser support | All system tests pass in each of the latest two versions of Chrome, Firefox, Edge, and Safari | Any system test fails in any supported browser | REQ-01, REQ-02 | TC-1 to TC-6 (per browser) |

## Plan level

**Severity levels:**

| Severity | Definition |
|----------|------------|
| Critical | Crash or freeze, a failure shown as success, or any retention of frames or landmarks |
| High | An in-scope requirement is not met and there is no workaround |
| Medium | A requirement is met but with degraded behavior |
| Low | Cosmetic or minor issue with no effect on a requirement |

**The test plan passes if all of the following are true:**
1. All test cases have the status Pass. None are Fail, Blocked, or Not Run.
2. Every metric in the table meets its pass threshold.
3. There are no open critical or high defects.
4. System tests have been run and passed in each of the four supported browsers.

**The plan fails if any of the following is true:**
- Any condition above is not met at the final test run.
- Any Critical defect is open.
- Recognition accuracy is below 90%.

# Milestone
A table of milestone, test activity, owner, and evidence, placing each layer against your Week 8 milestones: unit and integration during sprints, system and acceptance before reviews and demos.


| Milestone | Test Activity | Owner | Evidence |
|-----------|---------------|-------|----------|
| **M1, Final Design Demonstration** (November 2026) | **Plan review.** Present the project plan. | Team | Test_Plan.pdf; instructor and TA feedback |
| **Sprint 1**, Tasks 1 to 4 | **Unit (about 10).** Landmark extraction with valid, no-hand, and partial-hand input (TC-7 to TC-9). Rest-interval detection at 0.5 s, 1 s, and 1.5 s, plus the 30 s cap (TC-10 to TC-12). Model loading and classification of known samples in the backend. Candidate-model benchmark recording size, speed, and accuracy (risks R1, R4, R5). | Braden (Input Processing); Connor (backend); Isaac (model benchmark) | Unit test report committed to the repository; console captures of landmark output; model comparison table |
| **Sprint 2**, Task 5 | **Integration (about 3).** Video Intake to Input Processing to ML Model, end to end for fingerspelling. Covers session start and stop (TC-4, TC-5), classification of known and unsupported input (TC-13 to TC-15), and backend-down handling (TC-30). | Isaac (Task 5 owner), with Connor and Braden | Integration test log; TC statuses updated in Test_Plan.md; defects logged with TC ID |
| **Sprint 3**, Tasks 6 to 10 | **Unit (about 14).** English text lookup and exact-match display (TC-16 to TC-18). Confidence threshold including exactly 0.80 (TC-19 to TC-21). Shape scoring, arrow direction, and the missing-reference case (TC-22 to TC-24). **Integration (about 2).** Backend result through the pipeline to the UI, timed against the display limit (TC-35). | Isaac (text translation); Braden (shape scoring); Connor (pipeline and confidence threshold) | Unit test report; integration log; first display-time measurements |
| **M2, Partial Prototype Test** (January 2027) | **System run 1 (partial scope).** Privacy cases first: no stored frames or landmarks (TC-32 to TC-34). Then permission and session cases (TC-1 to TC-6), TC-7 to TC-15, and TC-28 and TC-30. First prediction-time measurement (TC-44). Chrome only. | Each member runs the cases for their own component; Team for TC-1 to TC-6 and TC-32 to TC-34 | Results log with Status updated for every case run; TC-44 time and device recorded; defect list with no open Critical defects |
| **Sprint 4**, Task 11 | **Integration (about 3).** UI shows text and shape score together. Out-of-frame warning and recovery (TC-25 to TC-27). All five failure states and the silent-discard checks (TC-28 to TC-31, TC-41 to TC-43). | Isaac (errors, NFR-04); Team (REQ-09) | Integration log; screenshots of each error and warning state |
| **M3, Full Prototype Test** (February 2027) | **System run 2.** Full set TC-1 to TC-46 on Chrome. Browser-sensitive cases (TC-1 to TC-6, TC-26, TC-32, TC-35) on Edge, Firefox, and Safari. **ML trials:** 20+ attempts per sign, 3+ signers, 2+ lighting conditions, plus 20+ non-sign trials. Timing cases (TC-35 to TC-37, TC-44 to TC-46) on the minimum device. **Acceptance session 1:** advisor demo, including validation of shape feedback (REQ-08). | Connor (ML trials); Braden (REQ-03, REQ-04, REQ-08); Isaac (REQ-10, NFR-04); Team (browsers, privacy, timing) | Full results log; confusion matrix; timing log with slowest run per case; browser matrix; advisor feedback notes |
| **Sprint 5**, Tasks 12 to 17 | **Unit (about 6) and integration (about 2)** for the new full-sign endpoint, model switch, and translator outputs. New TC IDs for the full-sign model are written before the build and logged in the traceability matrix and the technical specification's version log. **Regression:** after every merge, rerun TC-13, TC-16, TC-21, TC-30, TC-35, and TC-32. | Isaac (endpoint, translator); Connor (pipeline); Braden (UI switch, translator outputs) | Unit and integration report; regression log per merge; updated Test_Plan.md and traceability matrix |
| **Task 18** | **System run 3.** Whole flow with fingerspelling and the small sign dataset. | All | Whole-flow results log; defect list |
| **M4, Full Test** (March 2027) | **System run 4.** Full set TC-1 to TC-46 plus new full-sign cases, in all four supported browsers. ML trials rerun for both models. Timing on the minimum device. **Acceptance sessions 2 and 3:** beginner learner and seasoned ASL learner, each followed by the post-session survey. Evaluate the plan-level pass criteria. | Team; Connor (ML trials); Braden (survey and acceptance sessions) | Final results log; survey forms with averages; defect log with 0 open Critical or High; completed plan-level pass and fail checklist |
| **Protected buffer** (3/29/27 to the Expo) | **Fix and retest.** Each failed case and its whole requirement group are rerun, and the full ML trial set is rerun if the model or thresholds changed. | Owner of the failing component | Defect closure log; retest statuses |
| **M5, Final Expo Demonstration** (April 2027) | **System run 5 (dress rehearsal).** Smoke set (TC-1, TC-4, TC-13, TC-16, TC-21, TC-26, TC-30, TC-32, TC-35) on the demo device, with venue-like network and lighting. Project-level definition of done checked. | Team | Smoke checklist; signed definition-of-done checklist; observation notes from audience use |


# Definition of done
A checklist a feature must meet before your team calls it complete, and a project-level version of the same: tests, review, documentation, open defects, and sign-off.

## Feature-level checklist

**Tests**
- Every test case traced to the feature's requirement ID has a Pass status.
- Each requirement has at least 3 cases, including at least one edge case or failure case.
- The unit tests for the feature's code pass on the current main branch, and the integration tests for its component interactions pass.
- Every metric the feature is tied to meets its pass threshold in the Pass and fail criteria section.
- If the feature touches webcam frames or landmarks, the retention cases (TC-32 to TC-34, TC-38 to TC-40) pass and 0 frames or landmark arrays are found in storage, memory, or logs.

**Review**
- The code was reviewed through a pull request by at least one teammate who did not write it, and every review comment is resolved.
- The change is merged into the main branch with no failing checks.
- Every failure path in the feature shows a user-visible message and none ends in a crash, freeze, or silent discard.

**Documentation**
- The code has comments or a short description explaining what it does.
- Test_Plan.md has the current Status for every affected case, with the measured values, browser, and device recorded.
- Any new, changed, or renumbered TC IDs are updated in the traceability matrix and logged in the technical specification's version log.
- Any thresholds, versions, or datasets the feature depends on are recorded in the repository.

**Open defects**
- There are 0 open Critical or High defects against the feature.
- Any open Medium or Low defects are logged with a description and either a planned fix or a documented deferral.

**Sign-off**
- The component owner confirms the feature meets its requirement and has all items above checked.
- A second teammate confirms the evidence: the test log, the metric measurements, and the reviewed pull request.

## Project-level checklist

**Tests**
- All test cases have the status Pass.
- Every metric in the Success Criteria section meets its pass threshold on the final run.
- System tests have been run and passed in all four supported browsers, and timing tests have passed on the minimum supported device.
- The ML trials were run on the final model and pass.
- The whole-flow test has passed on the final build.

**Review**
- All code in the main branch was merged by pull request with a teammate review.
- Licensing of every pretrained model and third-party dependency has been checked and recorded.

**Documentation**
- Test_Plan.md is final, with Status and measured values recorded for every case.

**Open defects**
- 0 open Critical or High defects.
- The defect log is complete, and each fixed defect lists the case that was rerun to confirm the fix.

**Sign-off**
- Each component owner has signed off on their own components.
- The team has agreed that the project is ready for the demo, and the smoke set passed on the demo device.
- The advisor has reviewed the results and has no unresolved concerns.


If you add or renumber TC IDs, update the Week 9 traceability matrix and log the change in the technical specification's version log.

Suggestions

Write the metrics table first. For each non-functional requirement and each numeric condition in a functional one, write the metric, the threshold, and the measurement method. A good metric almost writes its own pass and fail criterion.

Your D2 edge cases and error-handling behavior are your list of non-happy-path cases. Work through that list rather than inventing cases from memory.

Prioritize by risk. Core flows and anything touching data integrity are tested first, and the schedule should show that order.

Trade two test cases with another team. If they cannot execute your case without asking a question, the steps are not specific enough.

A metric with no requirement ID behind it is a wish, and a requirement with no metric is untestable.

Grading


Scope clarity	
In-scope and out-of-scope items listed specifically, by requirement ID and component, with reasons for the exclusions
20 points

Test case quality
At least 3 cases per major requirement in the five-column template, each tracing to a requirement ID, each group including an edge case or failure condition, with steps precise enough for a stranger to execute
35 points

Pass and fail criteria
Every expected result and every metric has a measurable, objective threshold; both pass and fail conditions are stated; the definition of done is explicit
25 points

Environment and schedule
Environment is reproducible in versions, data, and devices; the schedule places each test layer against real milestones with owners and evidence
20 points