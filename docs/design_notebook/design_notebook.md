


TDL - overall design notebook
- [x] list out weeks and dates
- [x] list out assignment due dates
- [x] list out commits
- [x] list out communications
- [x] piece together any empty ones - when did i push a discussion board post?
- [ ] meeting minutes for the one meeting we're missing, and for the meeting on 11/18/26
- [ ] add meetings






# Week 14 11/23/26 - 11/29/26

Assignment #15 Project Advisor Assessments due Wednesday 11/25/26

# Week 13 11/16/26 - 11/22/26

Assignment 13 Team Oral Presentation, Recorded Video, and Peer Reviews due Wednesday 11/18/26

Assignment 12 Presentation Draft and Documented Advisor Feedback due Wednesday 11/18/26

commits
connor - advisor feedback template 11/16/26
connor - assignment 14 11/17/26

comms
11/16/26 - plan advisor feedback meeting, recording video, book room

# Week 12 11/9/26 - 11/15/26

Assignment 11 Ethics and Responsible Use Statement due Wednesday 11/11/26

commits
connor - ethics statement 11/9/26
isaac - ethics statement 11/11/26
braden - ethics statement 11/11/26

connor - runsheet md 11/15/26

comms
11/9 - a11
11/10 - init slideshow
11/11 - finalize a11


# Week 11 11/2/26 - 11/8/26

Assignment 10 Test and Validation Plan Draft due Wednesday 11/4/26

commits
isaac - test plan 11/4/26
braden - test plan - 11/4/26

comms
11/2, 11/3 - a10

# Week 10 10/26/26 - 11/1/26

Assignment 9 Requirements Document and Technical Specification Document due Friday 10/30/26 

Assignment 8 Task List, Milestones, Timeline, and Effort Matrix due Wednesday 10/28/26

commits
connor - task list final 10/26/26

isaac - req doc 10/30/26
braden - tech spec 10/30/26

connor - test plan.md 10/31/26

comms
10/26 - finalize a8
10/27 - a9
10/30 - finalize a9


# Week 9 10/19/26 - 10/25/26

commits
connor - task list 10/25/26

comms
10/20 - new git repo/org
10/21 - plan for ausiano meeting on 10/21
10/23 - connor does a8, braden and isaac team up on a9

# Week 8 10/12/26 - 10/18/26

Assignment 7 Detailed System Design D2 and Peer Review Notes due Wednesday 10/14/26

commits
connor - d2 10/12/26
isaac - d2 10/12/26
braden - d2 10/13/26

connor - peer review 10/14/26
isaac - peer review 10/14/26

comms
10/12 - d2, brainstorm how to do peer review
10/14 - peer feedback



# Week 7 10/5/26 - 10/11/26

Assignment 6 Detailed System Design D1 due Thursday 10/8/26

commits
isaac - updated d0 block diagram 10/7/26
connor - d1 10/7/26

connor - finish d1 10/8/26

comms
plan for meeting on 10/7/26
10/8 share thoughts about d1 / ml model



# Week 6 9/28/26 - 10/4/26

Assignment 5 High-Level System Design D0 due Friday 10/2/26

Assignment 4 User Stories and Use Cases due Monday 9/28/26 

commits
braden, connor, isaac - d0 10/2/26

comms
mostly d0 10/2/26

# Week 5 9/21/26 - 9/27/26

commits
braden - user stories and use cases 9/24/26

comms
9/23 connor visit aurisano


# Week 4 9/14/26 - 9/20/26

Assignment 3 Advisor Selection and Design Constraints Essay due Friday 9/18/26

commits
connor - individual constriants essay 9/15/26

connor - team contract, design notebook, 9/9 meeting notes 9/16/26
braden - team contract contact 9/16/26
isaac - team contract contact 9/16/26

issac - design constraints essay 9/18/26

comms
9/16 aurisano agreed
9/18 team contract approved

# Week 3 9/7/26 - 9/13/26

Assignment 2 Team Formation and Project Description due Wednesday 9/9/26

commits
braden - project topic (project_description.md) 9/9/26

meeting on 9/9/26

comms
plan for first meeting 9/7 and 9/8
team contract draft 9/8
advisor brainstorming 9/10, 9/11
9/13 reach out to aurisano


# Week 2 8/31/26 - 9/6/26

Assignment 1 Professional Biography due Wednesday 9/2/26

commits
connor - Initial commit 8/31/26
connor - professional bio - 8/31/26

isaac - professional bio 9/2/26

braden - professional bio 9/3/26 

connor - project_description.md and individual capstone assessments/ 9/4/26

comms
group chat created 9/2/26
start thinking of list of ideas
look at a2/a3


8/31/26 - exchange idea documents with isaac
9/2/26? - recruit braden from class
9/2/26 - delete discussion board post

# Week 1 8/24/26 - 8/30/26

8/26/26? - create db post
8/27/26 - isaac reached out in response to discussion board





# 10/7/26 Meeting Minutes

### Attendance

Braden, Connor, Isaac

### Purpose

Talk about Assignment 5 (high level design), and start planning for Assignment 6 (due tomorrow) and Assignment 7 (due next Wednesday 10/14/126) (both detailed system design)


### Decisions

 - Assignment 5
   - We combined the components in A5 from 9 to 5 (Video Capture, Input Processing, ML Model, Results Processing, and Feedback/UI)
     - This will reduce future workload, and simplify the components that were pretty similar
   - ML model may need enough development to be considered internal
     - Braden said he found some models, but they may not have everything that we need, so we may need to augment them
     - don't want too many of our components to be external. currently 2/5
 - Design
   - try to use existing ML model if possible - haven't found one that 100% what we want - e.g. max 250 ASL signs
   - MediaPipe - library/framework for hand tracker, some models that utilize it
     - html5 media devices api as input
   - web assembly - we can use this for higher processing for web browser stuff
   - we are settling on web browser for finalized deployment
     - don't have to worry about confusing OS system calls, don't have to sacrifice portability, seems to be relatively straightforward in terms of video capture, fits well into a client/server approach, gives us options in terms of either all javascript, or javascript and some backend (e.g. python)
   - PWA - multithreaded web app in browser - offline local application without .exe
   - github pages link to download everything and run on device - serverless
   - probably don't need to deploy it in cloud ever unless we love the result

### Tasks

 - Assignments 6 
   - for A6, ML Model component is best
     - it will probably have a data component and algorithms that are asked by the assignment
   - I can do ML model by tomorrow
   - Isaac has some time in hte evening if i don't finish
   - Braden can help a little with research
   - get into submittable but not perfect state by tomorrow
 - Assignment 7
   - Braden can do send feedback and Isaac can do results processing
     - these assignments are not strict
   - I will continue to polish/expand ML Model
   - for external components - last, and whoever finishes their components first will do those
   - aim to finish A7 stuff by next wednesday
     - meet around same time
 - Aurisano
   - we talk to Aurisano a little a week after next week (10/21/26, ~220pm), and meet her and potentially schedule a meeting
     - office hours are right after class monday/wednesday
     - class is UI
 - Other tasks
   - i can fix the interfaces table for v2 - but low priority
   - i can create a github org and migrate repo to their - sometime soon
   


# 9/9/26 Meeting Minutes

### Attendance

All three team members  

### Purpose

Narrow down project topics and begin talking about advisor ideas  

### Results

We narrowed down to ASL to text translator framed as a tool to help people learn ASL (see topic statement)  

Runner up ideas include the Scheduler, or interactive calendar. Framed as an improvement to when2meet.com, it could move commitments around dynamically if plans change, would need integration with existing calendars (outlook, google, apple), maybe include AI optimizations by learning other people's calendars when something would be best to schedule, can create plans considering others class schedules, and could potentially read from Discord/Slack to dynamically update calendars based on messages. These were all possible ideas, and not features, but IIRC none of us were super stoked about it as it didn't really stand out compared to existing applications  

Another runner up idea was campus navigation, which would help people find their classes and find the best route between buildings, but the bearcat app / my bearcat network already has a tool to traverse buildings, and it would be very difficult to traverse between classes as it would either be scraping building plans or tediously walking around buildings, plus it would be difficult to deal with construction or changing routes or rooms moving and that kind of stuff, so it was bumped down

the budgeting app and skill gap app (take in resume or curriculum, scan local job listings, and determine what skills you should work on or how to improve your resume to better improve your job listings) was also notable, but we would have to compete with chatgpt in terms of functionality so it wouldn't be much of an improvement to something. 

over the next week we'll separately research and investigate advisors, then share via text what our ideas were then start reaching out, one at a time


(taken by connor on 9/16, not really sure what meeting minutes look like but yeah feel free to edit it)