# design leetcode

## func
- user should be able to get a list of problems
- get a specific problem
- submit a problem & get feedback
- Check the leaderboard for a competition

## OOS
- user profile
- GDPR & auth
- checking other solutions
- payment processing (PCI-DSS)

## Constraints
- competition users num 100,000 
- 4000 problems
- mostly < 200 submissions for each problem per user

## non-func
- PACELC: AP+L
- system should provide isolation & security when running the code
- seeing the list of problems & specific prob. high avail, low latency
- submission and result should be fast--not necessarily real-time

## Core entities
- User
- Problem
- Submission
- Leaderboard

## API design
// header includes authentication info like JWT or session token

GET /v1/problems?category={}&difficulty={} -> partial<problem>[]

GET /v1/problems/{problem_number} -> problem

POST /v1/problems/{problem_id}/submit -> submission
body {
    language
    code
}

GET /v1/leaderboard/{competition_id}?page={} -> leaderboard

## Design
Submission
- user_id
- problem_id
- code
- result
- competition_id

Problem
- id
- desc
- ...


## Things learned
- Security/isolation of a container
    - Talk through a port or return result to a control pane 
    - Allowlist very narrow (potentially 1) port
    - Used hardened base image with a baked in set of imports
    - Resource monitoring and termination 
    - Vuln management 
    - Run as custom user. No root
    - docker's "seccomp" option. secure computing
- Making leaderboard fetching efficient
    - using redis with sortedset DS. Something that looks like: "competition:leaderboard:{competitionId} {score} {userId}"
        - the key is the name of the sortedset then val, member (uniq)
    - when the app updates the DB, update Redis as well
    - Then the client can periodically poll or we can do SSE for the leaderboard 
- Heavier workload than usual for the long running OJ tasks
    - First, horizontal autoscaling of the containers (ECS as a simpler choice)
    - We can introduce a SQS in between the main service and the container pools broker so that we can smooth out the load
        - as soon as we introduce SQS, whatever workflow that initiated this would have to be asynced. That is, after the user have submitted, they would have to poll (or sse,WS) to check for result