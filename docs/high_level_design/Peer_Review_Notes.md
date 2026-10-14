# Peer design review notes. 
The feedback your team received from every reviewing team, converted into action items: ID, rubric criterion (completeness, feasibility, clarity, risk), the issue and the team that raised it, the action, an owner, a priority (must-fix, worth considering, out of scope for now), and a due date. Add a short summary of which criterion drew the most feedback and the top two issues you raised while reviewing others.

| ID | Short Name | Criterion | Issue | Reporter | Action | Owner | Priority | Due Date |
|---|---|---|---|---|---|---|---|---|
|1|Interface Table|Completeness/Clarity|Interface table seems outdated|LO, TH|Update interface table with new block design|Connor|Important, not urget|11/14/26|
|2|Interface Names|Clarity|Difficult to read interface references on IDs alone|LO|Add relevant names to interface table and update references in diagrams|Braden|Out of scope for now|12/1/26|
|3|Flowchart Interface Relationsihp|Clarity|Difficult to understand relationship between interfaces and flowcharts|LO|Update interface references in flowcharts with updated interfaces and verify clarity of diagrams|Isaac|Out of scope for now|12/1/26|
|4|Testing Frameworks|Completeness/Risk|No mention of testing framework/infrastructure|LO|Investigate different testing frameworks and determine which are most feasible for this project|Connor|High priority|10/25/26|
|5|Condense Goal Statement|Clarity|Goal statement is too wordy|LO|Identify core part of goal statement and add conditions as sub points|Braden|High priority - quick fix|10/25/26|
|6|Formatting of Design|Clarity|Long blocks of texts and struggle to identify entities|LO|Separate long blocks of texts in design docs with paragraphs, add backticks around entity references|Isaac|Important, not urgent|11/14/26|
|7|Minimum Specs|Feasibility|What devices are/aren't going to be able to process for the model?|LO|Investigate/Identify minimum requirements of user devices to be compatible with the application|Connor|High priority|10/25/26|
|8|Legality|Feasibility|Possible legal items are relevant for public facing website|LO|Investigate legal items that need implemented (privacy policy, TOS, cookies, etc.) for a public-facing website|Isaac|High priority|10/25/26|
|9|Acceptance Criteria|Completeness/Clarity|When does the app know when the sign is finished? What does "relatively fast" mean for the ML model?|TH|Add specific acceptance criteria (that we've already established) into design docs|Braden|Medium|10/25/26|
|10|Specific ML Model|Completeness|What kind of ML model are you going to use? This part feels unfinished.|TH|Research options for the ML model more fully and implement|Connor|High Priority, long term|02/01/27|
|11|D1 & D2 Inconsistencies|Clarity|Inconsistencies in describing the model's output between D1 core algorithms, API, and D2 results processing|TH|Correct inconsistencies and update earlier redesigned elements|Isaac|High Priority|10/25/26|
|12|Scope|Feasibility/Clarity|Will there be any guided ASL learning or is this just an application for practice? If just practice, maybe rename the project from ASL *Learning* Assistant?|TH|Discuss as team|Team|Important, not urgent|11/01/26|


**Which drew most feedback:** We received plenty of **Clarity** suggestions, most of them stemming from the fact that our earlier design documents have become out-of-date the more desigining we did on a more granular level. These are mostly easy fixes of returning to the earlier higher level documents and updating what has changed.

**Top two issues while reviewing others:** Similarly, we also raised a few **Clarity** issues. Many of these were cases where the different design documents felt disconnected and needed to be updated or supplemented to flow together as one cohesive design. Another issue that came up frequently were **Completeness** issues; specifically, these were issues where pieces of the algorithm or edge cases or even the entire system were unstated in the design documents because they were assumed or written somewhere else that we didn't see. 
