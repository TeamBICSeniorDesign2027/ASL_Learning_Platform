# Design D1: ASL Learning Assistant

Team: Braden Monnin, Connor Slutsky, Isaac Dowdy

**Goal:** give self-taught ASL learners English text and hand-shape feedback from a webcam, without storing their video data.

**Input:** live webcam video of the learner performing an ASL sign.
**Output:** English text translation of the sign, plus feedback on the user's formation of the sign.

**Overview:** This system is intended to help learners validate their ASL signs. It'll be a web application such that a user will form their signs to their webcam, which will send their signs to an AI/ML model to guess what those signs are, how confident it is in that guess, and determine the accuracy of the signs. In D1, we covered the ML Model componenet. D2 will add the remaining components, edge cases, requirements traceability, design rational, and peer review. 


# Header, conventions, and D2 overview. 
~~Your project title and goal statement on every diagram,~~ with each diagram's conventions explained: what boxes mean, what lines represent. ~~Then a two-minute overview for a reviewer with no context: what the system is, what D1 covered, and what D2 adds.~~ List known gaps and open questions here rather than letting reviewers find them.


# Finished components, the D2 diagrams. 
For every component not fully detailed in D1: a data model (or the words "no persistent data"), its behavior as an algorithm entry, state machine, or step list, and its interface as endpoints, message formats, or signatures with errors. Give the glue components (authentication, notifications, configuration, logging) the same treatment. Revisit the D0 interfaces and revise any that grew too many parameters or leak internals.

# Edge cases and error handling. 
One table with columns for ID, component, category, the edge case, and the design decision. Cover all four categories from class for each component: boundary values, timing and concurrency, bad or missing input, and external dependencies. Every row needs a decided behavior (retry, degrade, fall back, or surface an error), a named handler, and a note on whether it is recoverable. At least one diagram must show error paths, not only the happy path.

# Requirements traceability table. 
One row per requirement, functional and non-functional: ID and short text, priority, the design elements that satisfy it (an entity, endpoint, algorithm, or figure, not a component name alone), and how. Explain every gap; never leave a cell blank. Run the check in reverse too, because a component with no requirement behind it is scope creep.

# Design rationale. 
Six to 10 entries for the significant D1 and D2 decisions, each in the form "We chose X over Y because ___. We accepted the trade-off of ___ because ___." Mark the decisions the peer review changed, and reference the D0 decision log rather than repeating it.


# Peer design review notes. 
see Peer_Review_Notes.md

# Suggestions
- Walk the D0 diagram box by box before class and assign what is missing to owners. That audit is faster than rediscovering the gaps in the review.
- Do not skip an edge-case row because the answer is hard. That row is where the reviewers will focus.
- Draw the decisions and failure outcomes of your riskiest flow, and a state machine for anything with a lifecycle.
