# behavioral

Conflict
## Story1 HEXAGONAL
S-For a new service. One of the leads was the one pushing for hexagonal architectural pattern, and I did a lot of research regarding it and found the lead's project skeleton initialization inconsistent. 
A-I was the one in the team to point out the inconsistencies in his approach in module imports and how it breaks the pattern. It started as a thread in slack, but the thread was getting long, so I suggested we move to a call and pulled out a virtual whiteboard to draw out the architecture. Once we drew it out, the issues that I brought up became clear, and we were able to reach an agreement for this project quickly. Not only that, based on our discussion, I enforced the architectural import rules using import linter.
R-In the end, we now have a robust decoupled architecture for this newborn project that several engineers are working on, and the rules are enforced programmatically. 

## Story2 RPM
S-One of the architects. high level architectural document describing the flow of the new way of managing cloudhsm clusters. 
creating backups of the clusters in a safe environment and deploying them in my team's aws accounts. Just like a docker image deployed with kubernetes. 
One of the leads in the client team had a major disagreement. Suggested RPM style. 
A-Architect stopped responding on that thread, so I took the opportunity to explain.
At first, I was further elaborating the pros and cons of the Architect's approach compared to the RPM approach. 
Later explained that the leadership wanted a clear ownership separation. Ultimately what happens with the deployed HSMs will be our responsibilities. 
R-Broke the deadlock. He agreed. Signed off.
L-When evaluating decisions, when there are multiple pros and cons for diff approaches, it's important to go deeper into the foundational constraints and goals we're facing

Perseverance
## Story1 REVIEW PERSERVERANCE
S-cross team PR reviews in a single channel. Supposed to provide an overview of what's going on in different teams. Lazy review requests. 
A-Asked the principal to uplift our engineering culture and there are way too many "please review" posts with bare minimum descriptions. 
The principal was appreciative of my feedback and told me he'll DM folks individually and that if it doesn't improve, he'll bring it up to the leadership. 
We saw a temporary increase in the standard, but it drifted back. 
So I brought it up to the principal again and suggested an escalation to the leadership level, which he did.
In the following team meeting, the need for PR description was raised. Assume it was the case for all other teams
R-now without having to click into each PR, one can scroll through the channel and see the sequence of PRs indicating the progress of each project
L-Don't be afraid to speak out your opinions when you see a potential improvement in the engineering culture

Adaptability
## Story1
Situation: Architect (zhou)'s disagreement on how the architecture is presented for cross-team review
Heated argument with architects about making a customer experience doc. KMS team
Customer-centric diagrams and documents (customer experience document)
## Story2
Situation: Creating a functional testing environment quickly that was overdue

Growth
## Story1
Situation: Envelope encryption wrong assumption
Details: Assuming things without gathering all the possible information. At that point, I wanted to prove that I can work on things on my own with minimal guideline, so I implemented something without envelope encryption and didn't fit with the input crypto material. A few days delayed, but because the due time was still a couple weeks ahead. Saved. But lesson learned that being proactive and being able to work on things on my own don't mean that I can go work on it without gathering as much information as I could. On a personal note, being proud is a very dangerous state to be in LL: Yes, you have to be able to work in ambiguity, but that's not an excuse to not gather as much information as possible before approaching a problem 
Collaboration
## Story1
Situation: Working on the cloudhsm bootstrap refactoring work where I had to collaborate with three other entities and was the glue of all. Mentored another Junior engineer for the infrastructure related portion because we have a peculiar system in our team due to the nature of HSMs. 
Impact: New org's PKI system build. A solid foundation for the scalable deployment and efficient cluster management 

Results
## Story1
Situation: Resolving a concurrency bug that was causing 173 false positive alerts per year
## Story2
Situation: Improving the CI process for the team's flagship service by eliminating redundant process.
Details:  Tired of waiting and checked what's actually going on with the process. Noticed that the makefile was setup in a way such that the unit test was running both in the unit test stage and the build stage. Because we have a comprehensive tests, the tests takes a long time. I've removed the dup process and was able to pull down the CI pipeline time from 30 something minutes to low 20s. 
