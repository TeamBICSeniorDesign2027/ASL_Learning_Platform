
What to submit

Your advisor and the instructor assess progress against this plan through CS 5002: Senior Design Project II, and the Week 14 report reproduces these tables. Submit one PDF with the four graded components in order, followed by the two supporting artifacts. Commit Tasklist.md to your repository root and the plan sources to a Planning folder.

# Task List (Assignment #5). 
Ten to 25 numbered tasks, one statement each, each beginning with a verb (specify, research, investigate, design, develop, obtain, document, test, validate, refine), naming the Design Diagram module it concerns, and ending with the one team member of primary responsibility. Aim for about five tasks per member, each sized to one or two weeks. Use the exact names from Project-Description.md and D0 to D2. Save the same list as Tasklist.md in the repository root.




| No | Task | Component | Team member | 
| ---| --- | --- | --- | 
| 1 | Implement test MediaPipe prototype | Video Intake/Input processing | Braden |
| 2 | Research and identify best fingerspelling models | ML Model | Isaac |
| 3 | Build backend with fingerspelling model | ML Model | Connor |
| 4 | Transform MediaPipe output to match fingerspelling model | Input Processing | Braden |
| 5 | Test fingerspelling flow | Video Intake - ML Model | Isaac | 
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


# Milestones List (Assignment #6). 
Every planned major milestone and completed deliverable with a detailed explanation: what it demonstrates, its date, how it is verified, and by whom. Include the completed course deliverables marked complete, the remaining ones through Week 14, and internal milestones every one to three weeks through the implementation semester. A milestone is a checkable, dated outcome, not a task.

| ID | Milestone | Description | Date | How Verified | By Whom |
| --- | --- | --- | --- | --- |
|M1| Final Design Demonstration | Oral presentation for end of Senior Design 1 | December 2026 | Sound description and design plans of project | Instructor, TA, Others in audience |
|M2| Prototype Test (Internal) | Test of prototype featuring all 5 components with fingerspelling dataset | January 2027 (after winter break) | Internally, through direct testing |  Each team member |
|M3| Full Test (Internal) | Test of full flow with fingerspelling + small sign dataset | March 2027 | Internally, through direct testing | Each team member | 
|M4| Final Expo Demonstration | Demonstration of final flow at UC Expo | TBA, Presumably April 2027 |Through audience participation in tool | Judges, Instructor, other expo-goers |

# Table 1, Timeline (Assignment #6). 
One row per Task List item with the task ID, statement, owner, start date, completion date, and dependencies, plus a row giving each milestone's planned completion date. Show your buffer explicitly: 15 to 20 percent per task plus a protected block before each major milestone. A Gantt figure is welcome in addition, but the table is the graded item.

| Task No | Task | Owner | Start | Completion | Dependencies |
| 1  | Implement test MediaPipe prototype | Braden|START|COMPLETE|DEP|
| 2  | Research and identify best fingerspelling models | Isaac |START|COMPLETE|DEP|
| 3  | Build backend with fingerspelling model | ML Model | Connor |START|COMPLETE|DEP|
| 4  | Transform MediaPipe output to match fingerspelling model | Braden |START|COMPLETE|DEP|
| M1 | Final Design Demonstration | Milestone | December 2026 | December 2026 | DEP
| 5  | Test fingerspelling flow | Isaac | START|COMPLETE|DEP|
| M2 | Prototype Test (Internal) | Milestone | January 2027 (after winter break) | 
| 6  | Build pipeline to output backend results in UI |Connor |START|COMPLETE|DEP|
| 7  | Research and design ASL to text translator component | Braden |START|COMPLETE|DEP|
| 8  | Build ASL to text translator component | Isaac |START|COMPLETE|DEP|
| 9  | Research and design sign accuracy determiner component |  Connor | START|COMPLETE|DEP|
| 10 | Build sign accuracy determiner component | Braden |START|COMPLETE|DEP|
| 11 | Refine UI to output ASL to text translation, and sign accuracy determination | Isaac |START|COMPLETE|DEP|
| 12 | Research and identify best full-sign models | Connor |START|COMPLETE|DEP|
| 13 | Refine UI to include button switch for UI and full-sign models |  Braden | START|COMPLETE|DEP|
| 14 | Refine backend to include endpoint for full-sign model |  Isaac |START|COMPLETE|DEP|
| 15 | Build pipeline to output backend results of new endpoint to UI |Connor |START|COMPLETE|DEP|
| 16 | Refine ASL to text translator component to include outputs of new model | Braden | START|COMPLETE|DEP|
| 17 | Refine text translation component to include outputs of new model |  Isaac |START|COMPLETE|DEP|
| 18 | Test whole flow | All |START|COMPLETE|DEP|
| M3 | Full Test (Internal) | Milestone | March 2027 | March 2027 | Dep
| M4 | Final Expo Demonstration |Milestone | TBA, Presumably April 2027 | March 2027 | Dep



# Table 2, Effort Matrix (Assignment #6). 
One row per task and one column per member, with effort in hours or percent that meaningfully represents the expected division of labor. Mark the one member of primary responsibility for each task and make sure that member carries the largest share. Add row and column totals. If the columns are uneven, say what you will do about it.

|Task|Braden|Isaac|Connor|
|---|---|---|---| 
| 1 - Implement test MediaPipe prototype | Braden|ISAAC|CONNOR|
| 2 - Research and identify best fingerspelling models | Isaac | ISAAC|CONNOR|
| 3 - Build backend with fingerspelling model | ML Model | Connor | ISAAC|CONNOR|
| 4 - Transform MediaPipe output to match fingerspelling model | Braden | ISAAC|CONNOR|
| 5 - Test fingerspelling flow | Isaac |  ISAAC|CONNOR|
| 6 - Build pipeline to output backend results in UI |Connor | ISAAC|CONNOR|
| 7 - Research and design ASL to text translator component | Braden | ISAAC|CONNOR|
| 8 - Build ASL to text translator component | Isaac | ISAAC|CONNOR|
| 9 - Research and design sign accuracy determiner component |  Connor |  ISAAC|CONNOR|
| 10 - Build sign accuracy determiner component | Braden | ISAAC|CONNOR|
| 11 - Refine UI to output ASL to text translation, and sign accuracy determination | Isaac | ISAAC|CONNOR|
| 12 - Research and identify best full-sign models | Connor | ISAAC|CONNOR|
| 13 - Refine UI to include button switch for UI and full-sign models |  Braden |  ISAAC|CONNOR|
| 14 - Refine backend to include endpoint for full-sign model |  Isaac | ISAAC|CONNOR|
| 15 - Build pipeline to output backend results of new endpoint to UI |Connor | ISAAC|CONNOR|
| 16 - Refine ASL to text translator component to include outputs of new model | Braden |  ISAAC|CONNOR|
| 17 - Refine text translation component to include outputs of new model |  Isaac | ISAAC|CONNOR|
| 18 - Test whole flow | All | 


# Work breakdown structure. 
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

# Risk register. 
At least five risks across the technical, team, and external categories, each with a likelihood, an impact, a strategy (avoid, reduce, transfer, accept), a mitigation, an owner, and a trigger. High-impact schedule risks should be visible in Table 1.

- technical - model too big 
- team - skill gap in implementing ASL model
- external - licensing issue with pretrianed ASL mdoel
- technical - model too slow
- technical - model not accurate enough

| ID |Risk | Description | Category | Likelihood | Impact | Strategy | Mitigation | Owner | Trigger |
|---|---|---|---|---|---|---|---|---|---|
| R1 | Model Too Large | Size of ML Model(s) are significant enough to warrant memory usage concerns, especially if deployed over cloud | Technical | Medium | High | Reduce | In research and analysis of model contenders, measure and record size of model. Use size as factor in determining model. Research maximum memory allowed for free Azure subscription | Owner | Discovery of models size conflicting with cloud deployment budget |
| R2 | Skill Gap in Implementation of Components | Due to trade-off of tasks (each member working each component), and wide skills and experience of team can lead to uneven development of components | Team | Medium | Low | Reduce | Proper documentation, code comments, and communication about functionality of designed components before passing them off. Encouraged 'training' when passing components off to the next person | Owner | Trigger |
| R3 | Incompatible Licensing with Pretrained ASL Model | Using pre-trained ASL model in a way that it violates its license | External | Low | High | Avoid | Proper investigation of model licenses before implementing into application flow to ensure proper usage of dependencies | Owner | Trigger |
| R4 | ML Model Too Slow | ML Model taking signficiant amount of time to predict user sign inputs | Technical | Medium | High | Reduce | Factor in model speed when researching contender models, set maximum allowable processing speed to use in decision-making process | Owner | Trigger |
| R5 | ML Model Insufficient Accuracy | ML Model not capable of determining signs with appropriate accuracy - e.g. model doesn't recognize any sign that the user signs, including simple signs | Technical | Medium | High | Reduce  | Set accuracy goal when researching for models, use accuracy as factor in choosing best model | Owner | Trigger |


 


# Grading

Task List (#5)
Ten to 25 verb-first, project-specific tasks, one statement and one primary owner each, about five per member, sized to one to two weeks, names consistent with the design diagrams, Tasklist.md in the repository
	
Milestones List (#6)
Every planned milestone and completed deliverable with an explanation, date, and verifier; spaced every 1 to 3 weeks; completed items marked
	

Table 1: Timeline (#6)
Every Task List item with a start and completion date and milestone completion dates; dependencies, critical path, and buffer visible
	

Table 2: Effort Matrix (#6)
Effort per member per task, consistent with the one primary owner; row and column totals; imbalance identified with a rebalancing plan
	

Overall plan quality
Consistent across Task List, work breakdown structure, timeline, effort matrix, and risks; mitigations visible in the timeline