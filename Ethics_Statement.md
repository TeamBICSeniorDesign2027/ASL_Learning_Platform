# What to submit
One document, Ethics_Statement.md, target 3 to 5 pages, committed to your repository root beside the Week 9 and Week 10 documents and submitted as a PDF. Your documentation lead owns it, and each section is checked by the member who owns the relevant component. Use the five part names below as section headings, and link the file from README.md section 5.

# Privacy summary. 
*A table with one row per data item: data item, source, purpose, consent, storage, access, and retention. Cover every field, log, sensor reading, and upload that crosses an arrow on your D0 to D2 diagrams. Say how consent is obtained and what happens to the data when the project ends. Name any field you removed because you could not justify it.*

|Data Item|Source|Purpose|Consent|Storage|Access|Retention|
|---------|------|-------|-------|-------|------|---------|

# Security summary. 
*How you addressed the three common capstone gaps: default credentials, unauthenticated endpoints, and secrets in source control. Name the mechanisms (authentication method, token lifetime, where secrets live, HTTPS, rate limiting), give the result of a search of your git history for committed secrets, and add a short table of the self-audit questions with your answers.*

 - default credentials
 - unathenticated endpoints
 - secrets in source control

mechanisms, secrets in git history, 

Self Audit:
|Self Audit Questions|Answers|
|--------------------|-------|

# Licensing summary. 
*A table of every third-party dependency, dataset, and creative asset: name, version, license (SPDX identifier), permissive or copyleft, and the obligation it places on you. State your project's own license, confirm it is compatible with every row, flag anything copyleft, and say what you decided.*

|Dependency Name|Version|License (SPDX)|Permissive/Copyleft?|Obligation|


Our license:


# Attribution list. 
*Libraries with source URLs, code adapted from tutorials or other repositories with the source named, datasets with collection method and usage restrictions, and non-code assets such as icons, fonts, and images with author and license. Put the same list in a README section or a NOTICE file.*

 - attribute


# AI-use disclosure. 
*Open by quoting the syllabus policy: "Generative AI tools may be used for drafting and code assistance but not for the design decisions or written analysis that are assessed in this course." Then record every use in the table below, sorted with the two-column framework from class. End with one signed line per member confirming that the architecture, data model, priorities, and design rationale were the team's own.*

"Generative AI tools may be used for drafting and code assistance but not for the design decisions or written analysis that are assessed in this course."

|Tool|Used by|Task|Column|How verified|
|----|-------|----|------|------------|
|GitHub Copilot|Member name|Boilerplate for X|Generally OK|Reviewed line by line; unit tests pass







For this project, the architecture, data model, priorities, and design rationale were the team's own

Signed,
Connor Slutsky,









Suggestions

Build the dependency and asset list from the actual files (package.json, requirements.txt, platformio.ini, the assets folder) and look up each license on the SPDX list rather than from memory.

Run the security audit before you write it up. Search the history, not only the current files, and say what you fixed.

Be specific about storage. "Encrypted at rest on Neon, readable only by the API service role, deleted by the 30-day job" is an answer; "stored securely" is not.

Reconstruct AI use member by member. If a use falls in the "not a substitute for" column, say what you did about it.

Consent is not implied by use. Say where and how a user is asked, and what they are told.


Grading 

Privacy and security
Specific, accurate answers for every data flow, the three common security gaps addressed by name, the git history searched, and retention and deletion described as mechanisms rather than intentions
30 points


Licensing and attribution
Every dependency, dataset, and asset listed with its license and the obligation it creates; your own license stated and compatible; copyleft decisions recorded; attribution in the repository, including adapted snippets
30 points


AI-use disclosure
An honest account by member and task, sorted with the two-column framework, each use stating how the output was verified, with a confirmation line signed by every member
25 points


Clarity and completeness
All five sections present and in order, specific to the project, cross-referenced to the Week 9 schema and stack table, and consistent with the Week 3 constraints essay
15 points