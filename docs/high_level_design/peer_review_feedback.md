# from LO
**Note**: LO put more emphasis on the structure of the design docs, which was a result of miscommunication. Nevertheless, still useful points. 

## Nits:
- If draw.io functional block diagrams need updating over time, consider embedding a mermaid diagram in the readme. It makes updating functional block diagrams/flowcharts a bit easier.  (see: https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-diagrams or https://mermaid.ai/open-source/intro/)
- Context of design docs are sprawling a little. I'd prefer smaller scoped chunks to keep your documentation precise. Have some linking parent document if you want to talk about the nuts & bolts of a feature.
- When referencing an item in documentation, wrap in `` or italics so I know that you're talking about something specific (ex. If referencing a component/interface from their respective tables). 
- Use of "I" in group project. "I" -> "we" or something.
- WIP stuff could be on a PR rather than main

## D0
### Completeness
- Completeness is fine. From a "high-level-design", I do think you are getting too detailed about interface documentation, though. I would probably want to see the interfaces stashed away to their own .md and have not have the flow charts reference the interfaces. Reading the documentation is tricky because I think it captures varying level of scope.
### Feasibility
- Scope is defined in a manner to make this project feasible. Using existing tooling to make life easier.
  - Not 100% sure what the plans are for handtracking, but consider using an off-the-shelf package (unless the goal is to learn)
- Testing frameworks?
### Clarity
- It might make sense to give the interfaces relevant names over Ids. I want to know what the interfaces do without reading the description. In source I assume there will be proper names, but it is difficult to gather what is being referenced in the documentation.
  - Interface referenced before it was defined in the table.
- The "goal" is pretty wordy. Maybe strip out some of the constraints you're putting onto the main goal and have those listed as separate. I'm interpreting your goal roughly as "ASL learning platform" with constraints of text feedback, ASL accuracy/correctness feedback, and data privacy. This is likely a preference thing though; I don't think there is a wrong/right answer here (regardless, I think the goal could be tweaked or rephrased a little). 
  - "without storing their video data." feels vague. Maybe I would prefer something like "client-side model" or something. Idk. But again, this feels like a constraint.
- Occasional sprawling blocks of text
- When referencing something in documentation, wrap in backticks `` or italics so I know you
### Risk
- Will the device always be able to run the model? Minimum specs of device needed?
  - Can you pivot to having the browser POST raw hand geometries? Is the issue with data collection the image themself?
- Are there any legal requirements you have to satisfy? For having a public-facing website.
  - Ex. Privacy policy, tos, cookie stuff, etc.
- Kudos for reducing size of model 

## D1
### Completeness
### Feasibility
### Clarity
- Pretty technical for a high-level overview
### Risk
- Testing frameworks?


AC
 - Polish interface table - assign names
 - Have flow charts reference the interfaces
 - investigate testing frameworks
 - Reword goal
 - Separate out blocks of text
 - Backticks when referencing entities
 - Investigate specs of device needed to run local models
 - Investigate legal requirements
