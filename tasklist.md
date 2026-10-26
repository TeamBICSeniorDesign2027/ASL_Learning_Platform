# Task List (Assignment #5)
Ten to 25 numbered tasks, one statement each, each beginning with a verb (specify, research, investigate, design, develop, obtain, document, test, validate, refine), naming the Design Diagram module it concerns, and ending with the one team member of primary responsibility. Aim for about five tasks per member, each sized to one or two weeks. Use the exact names from Project-Description.md and D0 to D2. Save the same list as Tasklist.md in the repository root.




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


# Milestones List (Assignment #6)
Every planned major milestone and completed deliverable with a detailed explanation: what it demonstrates, its date, how it is verified, and by whom. Include the completed course deliverables marked complete, the remaining ones through Week 14, and internal milestones every one to three weeks through the implementation semester. A milestone is a checkable, dated outcome, not a task.

| ID | Milestone | Description | Date | How Verified | By Whom |
| --- | --- | --- | --- | --- | --- |
|M1| Final Design Demonstration | Oral presentation for end of Senior Design 1 | December 2026 | Sound description and design plans of project | Instructor, TA, Others in audience |
|M2| Partial Prototype Test (Internal) | Test of prototype featuring Input Processing, fingerspelling ML Model, and barebones Send Feedback/Results Processing | January 2027 (after winter break) | Internally, through direct testing |  Each team member |
|M3| Full Prototype Test (Internal) | Test of all 5 developed components with fingerspelling model/dataset | Feburary 2027| Internally, through direct testing | Each team member
|M3| Full Test (Internal) | Test of full flow with fingerspelling + small sign dataset | March 2027 | Internally, through direct testing | Each team member | 
|M4| Final Expo Demonstration | Demonstration of final flow at UC Expo | TBA, Presumably April 2027 |Through audience participation in tool | Judges, Instructor, other expo-goers |

# Table 1, Timeline (Assignment #6)
One row per Task List item with the task ID, statement, owner, start date, completion date, and dependencies, plus a row giving each milestone's planned completion date. Show your buffer explicitly: 15 to 20 percent per task plus a protected block before each major milestone. A Gantt figure is welcome in addition, but the table is the graded item.

**Note:** Buffer is built-in with consistent overestimated task durations + no inclusion of weekends in calculation, which will likely include worktime during actual development. Buffer includes space to mitigate risks. 

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



# Table 2, Effort Matrix (Assignment #6)
One row per task and one column per member, with effort in hours or percent that meaningfully represents the expected division of labor. Mark the one member of primary responsibility for each task and make sure that member carries the largest share. Add row and column totals. If the columns are uneven, say what you will do about it.

**Note**: Primary owner has significantly larger share of a task than others.

|Task|Braden|Isaac|Connor|ROW TOTAL|
|---|---|---|---| ---|
| 1 - Implement test MediaPipe prototype | 100%|0%|0%|100%|
| 2 - Research and identify best fingerspelling models | 0% | 100%|0%|100%|
| 3 - Build backend with fingerspelling model ML Model | 15% | 15%|70%|100%|
| 4 - Transform MediaPipe output to match fingerspelling model | 90% | 0%|10%|100%
| 5 - Test fingerspelling flow | 33% |  34%|33%|100%|
| 6 - Build pipeline to output backend results in UI |0% | 0%|100%|100%|
| 7 - Research and design ASL to text translator component | 80% | 20%|0%|100%|
| 8 - Build ASL to text translator component | 30% | 70%|0%|100%|
| 9 - Research and design sign accuracy determiner component |  20% |  0%|80%|100%|
| 10 - Build sign accuracy determiner component | 70% | 0%|30%|100%|
| 11 - Refine UI to output ASL to text translation, and sign accuracy determination | 10% | 80%|10%|100%|
| 12 - Research and identify best full-sign models | 0% | 0%|100%|100%|
| 13 - Refine UI to include button switch for UI and full-sign models |  90% |  0%|10%|100%|
| 14 - Refine backend to include endpoint for full-sign model |  0% | 70%|30%|100%|
| 15 - Build pipeline to output backend results of new endpoint to UI |0% | 30%|70%|100%|
| 16 - Refine ASL to text translator component to include outputs of new model | 90% |  10%|0%|100%|
| 17 - Refine text translation component to include outputs of new model |  0% | 100%|0%|100%|
| 18 - Test whole flow | 34% | 33% | 33% |100%|
|TOTAL|662%|562%|576%|

**Rebalancing Plan:** Braden is encouraged to ask for assistance from Connor or Isaac to take some of the workload off, and he is also encouraged to offer to trade tasks if his schedule is too demanding for his current workload. 

Proofcheck:
Braden: 100+0+15+90+33+0+80+30+20+70+10+0+90+0+0+90+0+34 = 662
Isaac: 0+100+15+0+34+0+20+70+0+0+80+0+0+70+30+10+100+33 = 562
Connor: 0+0+70+10+33+100+0+0+80+30+10+100+10+30+70+0+0+33 = 576

# Work breakdown structure
At least two levels deep, phases and then tasks, showing where each Task List item sits.

 1) Phase 1: Prototype Fingerspelling
    1) 1 - Implement test MediaPipe prototype 
    2) 2 - Research and identify best fingerspelling models 
    3) 3 - Build backend with fingerspelling model 
    4) 4 - Transform MediaPipe output to match fingerspelling model 
    5) 5 - Test fingerspelling flow 
    6) 6 - Build pipeline to output backend results in UI
 2) Phase 2: Prototype With Results Processing
    1) 7 - Research and design ASL to text translator component 
    2) 8 - Build ASL to text translator component 
    3) 9 - Research and design sign accuracy determiner component 
    4) 10 - Build sign accuracy determiner component
    5) 11 - Refine UI to output ASL to text translation, and sign accuracy determination 
 3) Phase 3: Full Application
    1) 12 - Research and identify best full-sign models 
    2) 13 - Refine UI to include button switch for UI and full-sign models 
    3) 14 - Refine backend to include endpoint for full-sign model
    4) 15 - Build pipeline to output backend results of new endpoint to UI 
    5) 16 - Refine ASL to text translator component to include outputs of new model 
    6) 17 - Refine text translation component to include outputs of new model
    7) 18 - Test whole flow 

# Risk register
At least five risks across the technical, team, and external categories, each with a likelihood, an impact, a strategy (avoid, reduce, transfer, accept), a mitigation, an owner, and a trigger. High-impact schedule risks should be visible in Table 1.

| ID |Risk | Description | Category | Likelihood | Impact | Strategy | Mitigation | Owner | Trigger |
|---|---|---|---|---|---|---|---|---|---|
| R1 | Model Too Large | Size of ML Model(s) are significant enough to warrant memory usage concerns, especially if deployed over cloud | Technical | Medium | High | Reduce | In research and analysis of model contenders, measure and record size of model. Use size as factor in determining model. Research maximum memory allowed for free Azure subscription | Braden | Discovery of models size conflicting with cloud deployment budget |
| R2 | Skill Gap in Implementation of Components | Due to trade-off of tasks (each member working each component), and wide skills and experience of team can lead to uneven development of components | Team | Medium | Low | Reduce | Proper documentation, code comments, and communication about functionality of designed components before passing them off. Encouraged 'training' when passing components off to the next person | Connor | Difficulty in developing components that were started by other team members |
| R3 | Incompatible Licensing with Pretrained ASL Model | Using pre-trained ASL model in a way that it violates its license | External | Low | High | Avoid | Proper investigation of model licenses before implementing into application flow to ensure proper usage of dependencies | Isaac | Discovery of licensing issues with external dependencies |
| R4 | ML Model Too Slow | ML Model taking signficiant amount of time to predict user sign inputs | Technical | Medium | High | Reduce | Factor in model speed when researching contender models, set maximum allowable processing speed to use in decision-making process | Braden | ML Model taking more than several seconds to make predictions |
| R5 | ML Model Insufficient Accuracy | ML Model not capable of determining signs with appropriate accuracy - e.g. model doesn't recognize any sign that the user signs, including simple signs | Technical | Medium | High | Reduce  | Set accuracy goal when researching for models, use accuracy as factor in choosing best model | Connor | ML Model rarely outputting predictions that match users intent |