# ASL Learning Platform

# Table of Contents

1) [Team + Abstract](#team--abstract)
2) [Project Description](#project-description)
3) [Use Stories and Design Diagrams](#user-stories-and-design-diagrams)
4) [Project Tasks and Timeline](#project-tasks-and-timeline)
5) [ABET Concerns Essay](#abet-concerns-essay)
6) [PPT Slideshow](#ppt-slideshow)
7) [Self-Assessment Essay](#self-assessment-essay)
8) [Professional Biographies](#professional-biographies)
9) [Budget](#budget)
10) [Appendix](#appendix)

# Team + Abstract

### Team
 - Braden Monnin, BS Computer Science + M.Eng. AI, monninbm@mail.uc.edu
 - Isaac Dowdy, BS Computer Science + M.Eng. CS, dowdyis@mail.uc.edu
 - Connor Slutsky, BS Computer Science, slutskcp@mail.uc.edu

### Advisor
 - Dr. Jillian Aurisano, Assoc. Professor, Computer Science, CEAS, UC, aurisajm@ucmail.uc.edu

### Abstract

ASL learning platform to help ASL learners practice signing and determine if their communication is effective. The learner to perform signs into their webcam, the web application would extract landmarks, and send the landmarks to a ML Model backend. The application would output the model’s predicted text, sign accuracy, and confidence from the learner’s hand signs so the learner can evaluate and adjust.


# Project Description

Beginner ASL learners often practice signing alone, with no way to know in real time whether they've formed a sign correctly. We propose a webcam-based computer vision tool that recognizes ASL fingerspelling and gives the learner instant feedback on accuracy, turning solo practice into guided learning.  


Also see [project_description.md](project_description.md)

# User Stories and Design Diagrams

### User Stories

US-01 (primary): As a self-taught ASL learner, I want to see an English text translation from the sign I just performed, so that I can confirm the system understood my intended meaning. 

US-02 (primary): As a beginner ASL learner, I want to receive visual feedback on my hand shape accuracy while practicing, so that I can correct my mistakes before they become ingrained habits.

US-03 (secondary): As a Deaf conversation partner, I want the system to catch improper finger positions and hand shapes, so that I can easily understand the learner when we communicate in real life.

US-04 (hidden): As a State Privacy Regulator, I want the application to immediately discard webcam frames from RAM after processing rather than saving the user's body and hand geometry, so that the software legally complies with strict state biometric privacy laws.

### Diagrams

![ASL Learning Assistant Block Diagram 2](/docs/high_level_design/images/asl_block2.drawio.png)


### Components

| Component | Purpose |
| --------- | ------- |
| Video Intake | Input webcam footage of user practicing signs | 
| Input Processing | Isolate landmarks from the webcam footage | 
| ML Model | Make sign predictions from the landmarks, output sign and confidence |
| Results Processing | Determine if confidence is high enough, and process sign accuracy from landmarks and predicted sign |
| Send Feedback | Outputs results to user, including predicted sign, confidence, and sign accuracy | 

### Traceability Matrix

| Req ID | Description | Design Component (D0/D1/D2) | Test Case |
| ------ | ----------- | --------------------------- | --------- |
| REQ-01 | Webcam permission check | D0: Frame Capture / Video Intake | TC-1, TC-2, TC-3  |
| REQ-02 | Display live view & stop session | D0: Frame Capture / Send Feedback | TC-3, TC-5, TC-6|
| REQ-03 | Extract landmark sequence | D2: Input Processing (MediaPipe) | TC-7, TC-8, TC-9 |
| REQ-04 | 1-second rest to complete attempt | D2: Input Processing | TC-10, TC-11, TC-12 |
| REQ-05 | Classify completed attempt |	D0/D1: ML Model | TC-13, TC-14, TC-15 |
| REQ-06 | Display English text | D2: Results Processing / Send Feedback | TC-16, TC-17, TC-18 |
| REQ-07 | Low-confidence warning message | D2: Results Processing (Confidence Check) | TC-19, TC-20, TC-21 |
| REQ-08 | Hand-shape accuracy feedback | D2: Results Processing (Shape Scoring) | TC-22, TC-23, TC-24 | 
| REQ-09 | Out-of-frame warning (> 1s) | D2: Input Processing | TC-25, TC-26, TC-27 | 
| REQ-10 | Explicit error handling | D2: All APIs / Send Feedback | TC-28, TC-29, TC-30, TC-31 | 
| REQ-11 | No persistent storage used | D1: Data Model (None) | TC-32, TC-33, TC-34 | 
| NFR-01 | Text displayed within 3s | D1/D2: ML Model + UI pipeline | TC-35, TC-36, TC-37 | 
| NFR-03 | Memory flushed after session | D1: Data Model / React unmount | TC-38, TC-39, TC-40 |
| NFR-04 | Errors are visible, not silent | D2: Send Feedback | TC-41, TC-42, TC-43 |
| NFR-05 | Prediction bound < 5s | D1: ML Model | TC-44, TC-45, TC-46 |

#### Also see: 
 - [User_Stories.md](User_Stories.md)
 - [D0](docs/high_level_design/design_d0_document.md)
 - [D1](docs/high_level_design/design_d1_document.md)
 - [D2](docs/high_level_design/design_d2_document.md)
 - [Requirements Doc](Requirements_Document.md)
 - [Technical Specification](Technical_Specification.md) 
 - [Test Plan](Test_Plan.md)

# Project Tasks and Timeline

### Task List

| No | Task | Component | Team member | 
| ---| --- | --- | --- | 
| 1 | Implement test MediaPipe prototype | Video Intake/Input processing | Braden |
| 2 | Research and identify best fingerspelling models | ML Model | Isaac |
| 3 | Build backend with fingerspelling model | ML Model | Connor |
| 4 | Transform MediaPipe output to match fingerspelling model | Input Processing | Braden |
| 5 | Test fingerspelling flow | Video Intake - ML Model | All | 
| 6 | Build pipeline to output backend results in UI | Send Feedback | Connor |
| 7 | Research and design ASL to text translator component | Results Processing | Braden |
| 8 | Build ASL to text translator component | Results Processing | Isaac |
| 9 | Research and design sign accuracy determiner component | Results Processing | Connor | 
| 10 | Build sign accuracy determiner component | Results Processing | Braden |
| 11 | Refine UI to output ASL to text translation, and sign accuracy determination | Send Feedback | Isaac |
| 12 | Research and identify best full-sign models | ML Model | Connor |
| 13 | Refine UI to include button switch for UI and full-sign models | Video Intake/Input Processing | Braden | 
| 14 | Refine backend to include endpoint for full-sign model | ML Model | Isaac |
| 15 | Build pipeline to output backend results of new endpoint to UI | Send Feedback | Connor |
| 16 | Refine ASL to text translator component to include outputs of new model | Results Processing | Braden | 
| 17 | Refine text translation component to include outputs of new model | Results Processing | Isaac |
| 18 | Test whole flow | Video Intake - Send Feedback | All |


### Timeline

| Task No | Task | Owner | Start | Completion | Dependencies |
|---|---|---|---|---|---|
| 1  | Implement test MediaPipe prototype | Braden|11/23/26|11/27/26|None|
| 2  | Research and identify best fingerspelling models | Isaac |11/23/26|11/27/26|None|
| 3  | Build backend with fingerspelling model ML Model | Connor |11/30/26|12/4/26|2|
| M1 | Final Design Demonstration | Milestone | December 2026 | December 2026 | None|
| 4  | Transform MediaPipe output to match fingerspelling model | Braden |12/7/26|12/11/26|1,3|
| 5  | Test fingerspelling flow | Isaac | 12/14/26|12/25/26|4|
| M2 | Partial Prototype Test (Internal) | Milestone | January 2027 (after winter break) | January 2027 | 5| 
| 6  | Build pipeline to output backend results in UI |Connor |12/28/26|1/8/26|5|
| 7  | Research and design ASL to text translator component | Braden |12/28/26|1/8/26|5|
| 8  | Build ASL to text translator component | Isaac |1/11/26|1/22/26|7|
| 9  | Research and design sign accuracy determiner component |  Connor | 12/8/27|1/8/26|5|
| 10 | Build sign accuracy determiner component | Braden |1/11/26|1/22/26|9|
| 11 | Refine UI to output ASL to text translation, and sign accuracy determination | Isaac |1/25/26|2/5/26|8,10|
| M3 | Full Prototype Test (Internal) | Milestone | Feburary 2026 | Feburary 2026 | 11 |
| 12 | Research and identify best full-sign models | Connor |1/25/26|2/5/26|8,10|
| 13 | Refine UI to include button switch for UI and full-sign models |  Braden | 1/25/26|2/5/26|8,10|
| 14 | Refine backend to include endpoint for full-sign model |  Isaac |2/8/26|2/19/26|12|
| 15 | Build pipeline to output backend results of new endpoint to UI |Connor |2/22/26|3/5/26|14|
| 16 | Refine ASL to text translator component to include outputs of new model | Braden | 3/8/26|3/19/26|15|
| 17 | Refine text translation component to include outputs of new model |  Isaac |3/8/26|3/19/26|15|
| 18 | Test whole flow | All |3/22/26|3/26/26|16,17|
| M4 | Full Test (Internal) | Milestone | March 2027 | March 2027 | 18 |
| M5 | Final Expo Demonstration |Milestone | TBA, Presumably April 2027 | April 2027 | 18|

#### Also see
 - [Task List](tasklist.md)


# ABET Concerns Essay

### Privacy Summary

All data related to the user - webcam footage, landmarks, predictions, resulting metrics, etc. will not be stored for longer than necessary. 

### Security Summary

If we choose to implement authentication and authorization, we will not incorporate default credentials, we can implement a basic form of MFA, we can implement API authentication with a JWT Token with an appropriate timeout, we will not commit secrets, we can configure HTTPS for extra security, and we can implement rate limiting to prevent DOS/DDOS attacks.

### Licensing Summary

The licenses we are using for all languages, frameworks, tools, and libraries we are using are all permissive and necessitates the inclusion of the licenses and appropriate notices in the repository. 

### AI-Use Disclosure

Generative AI tools may be used for drafting and code assistance, but was not and will not be used for design decisions or written analysis. In this project, the architecture, data model, priorities, and design rationale were all the team's own

### Design Constraints

Security - No storage of data beyond absolutely necessary for the functioning of this applicaiton

Ethics - The storage of data should be made clear to the user, explicit and informed consent should be given. We will avoid using external dependencies with known exploitable vulnerabilities. 

Diversity and Culture - We will design this application wtih clear feedback and usability to support the many types of users that can come to use this application. We will prioritize correctness of translation, appropriately filter inappropriate translations as necessary. 

Economic - The scope of this project necessitates spending as little as possible, and stresses the reliance on FOSS. 

#### Also See
 - [Ethics Statement](Ethics_Statement.md)
 - [Design Constraints Essay 1](/docs/individual_constraints_essays/individual_constraints_essay_connor_slutsky.md)
 - [Design Constraints Essay 2](/docs/individual_constraints_essays/dowdy_contraints_essay.md)

# PPT Slideshow

TODO

# Self-Assessment Essay

TODO 

# Professional Biographies

### Braden

TODO

### Isaac

TODO

### Connor

Connor has experience with C++, Python, and JavaScript from University courses, and gained experience with Go, React, Docker, Git, Kubernetes, Agile Scrum, Power BI, and other tools from Co-Ops at Honda and Radiance Technologies. 

#### Also see: 
 - [Braden Professional Biography](/docs/professional_bios/Monnin_Professional_Biography.md)
 - [Isaac Professional Biography](/docs/professional_bios/isaac-dowdy-bio.md)
 - [Connor Professional Biography](/docs/professional_bios/connor_prof_bio.md)

# Budget

There have been no expenses to date, nor donated items.

# Appendix

TODO - meeting notes

TODO - evidence of everyone contributing 45 hours 

### Links and References
 - [GitHub](https://github.com/TeamBICSeniorDesign2027/ASL_Learning_Platform)
 - [project_description.md](https://github.com/TeamBICSeniorDesign2027/ASL_Learning_Platform/blob/main/project_description.md)
 - [User_Stories.md](https://github.com/TeamBICSeniorDesign2027/ASL_Learning_Platform/blob/main/User_Stories.md)
 - [D0](https://github.com/TeamBICSeniorDesign2027/ASL_Learning_Platform/blob/main/docs/high_level_design/design_d0_document.md)
 - [D1](https://github.com/TeamBICSeniorDesign2027/ASL_Learning_Platform/blob/main/docs/high_level_design/design_d1_document.md)
 - [D2](https://github.com/TeamBICSeniorDesign2027/ASL_Learning_Platform/blob/main/docs/high_level_design/design_d2_document.md)
 - [Requirements Doc](https://github.com/TeamBICSeniorDesign2027/ASL_Learning_Platform/blob/main/Requirements_Document.md)
 - [Technical Specification](https://github.com/TeamBICSeniorDesign2027/ASL_Learning_Platform/blob/main/Technical_Specification.md) 
 - [Test Plan](https://github.com/TeamBICSeniorDesign2027/ASL_Learning_Platform/blob/main/Test_Plan.md)
 - [Ethics Statement](https://github.com/TeamBICSeniorDesign2027/ASL_Learning_Platform/blob/main/Ethics_Statement.md)
 - [Design Constraints Essay 1](https://github.com/TeamBICSeniorDesign2027/ASL_Learning_Platform/blob/main/docs/individual_constraints_essays/individual_constraints_essay_connor_slutsky.md)
 - [Design Constraints Essay 2](https://github.com/TeamBICSeniorDesign2027/ASL_Learning_Platform/blob/main/docs/individual_constraints_essays/dowdy_contraints_essay.md)
 - [Braden Professional Biography](https://github.com/TeamBICSeniorDesign2027/ASL_Learning_Platform/blob/main/docs/professional_bios/Monnin_Professional_Biography.md)
 - [Isaac Professional Biography](https://github.com/TeamBICSeniorDesign2027/ASL_Learning_Platform/blob/main/docs/professional_bios/isaac-dowdy-bio.md)
 - [Connor Professional Biography](https://github.com/TeamBICSeniorDesign2027/ASL_Learning_Platform/blob/main/docs/professional_bios/connor_prof_bio.md)