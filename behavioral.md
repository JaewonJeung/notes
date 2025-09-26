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
Details: 
Collaboration
Situation: 
## Story2
Situation: Working on the cloudhsm bootstrap refactoring work where I had to collaborate with three other entities and was the glue of all. Mentored another Junior engineer for the infrastructure related portion because we have a peculiar system in our team due to the nature of HSMs. 
Impact: New org's PKI system build. A solid foundation for the scalable deployment and efficient cluster management 

Results
## Story1
Situation: Resolving a concurrency bug that was causing 173 false positive alerts per year
## Story2
Situation: Improving the CI process for the team's flagship service by eliminating redundant process.
Details:  Tired of waiting and checked what's actually going on with the process. Noticed that the makefile was setup in a way such that the unit test was running both in the unit test stage and the build stage. Because we have a comprehensive tests, the tests takes a long time. I've removed the dup process and was able to pull down the CI pipeline time from 30 something minutes to low 20s. 


• Your reasons and motivations for changing roles through your career, and why you
found those roles and teams exciting.
• Clear descriptions of your role within the projects you’ve worked in, types of partners
you’ve interacted with, unique challenges you’ve encountered.
• Learnings from prior successes and failures and sharing what could have been done
differently to help optimize the way you approach these types of problems in the
future.
    - Architect (zhou)'s disagreement on how the architecture is presented for cross-team review Heated argument with architects about making a customer experience doc. KMS team Customer-centric diagrams and documents (customer experience document). Had to then understand some processes from the KMS side and gather requirements. In the process of doing so, I've also discovered some aspects that would not be feasible with cloudhsms--such as not being able to call create cluster or add HSM cross-aws account
• The ability to proactively identify issues, and to operate + learn independently.
    - smscps inter-process concurrency bug
• Understanding your approach to conflict resolution. How do you voice and receive
feedback? Did you have empathy for your cross-functional partners and their
perspectives? What was the outcome and common ground that came from these
experiences.
    - cloudhsm backup software architecture design and implementation. Hexagonal architecture. He wanted the file structure to be consistent even if that meant that the dependency graph is going to have some exceptions, and I argued for strict adherance to the architecture especially since we are just starting out a new project. After a few back and forth, I've decided to call him and get this sorted out. I've pulled up a whiteboard and explained. Agreed. And while he was revising his implementation, I've worked on setting up an import linter so that in the future, people won't accidentally break the architecture
• How do you approach ambiguous situations? We like to learn about the times you
had to navigate undefined environments without a playbook, and what you did to
pioneer major changes.
    - Sequence diagram of the flow to organize my though process and get feedbacks from it. Though as any ambiguous problems are, I've realized I had to perform envelope encryption to wrap a secret, and that added a couple more days to implement and test
• Move quickly: we want to be as agile as possible while being resourceful. Tell us
more about times that you were able to bump up deadlines, partnered through
efficiency and other optimization examples.
    - Noticing integration failure during integration testing for the cloudhsm bootstrap project. Instead of redoing the ceremony which involves 5 engineers' time, upon weighing the tradeoffs, I've decided to manually add a field for the pipeline trigger rather than doing the dev ceremony all over again
• Understanding what impact means to you, and what drives you to spend the time
doing what you do, particularly in your professional career.