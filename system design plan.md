# system design plan

- Read through the System design topics 
    - donnemartin https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#study-guide
    - hello interview https://www.hellointerview.com/learn/system-design/in-a-hurry/introduction
    - Alex Xu https://github.com/preslavmihaylov/booknotes/tree/master/system-design/system-design-interview
- Read through real world architectures techs
- Read through company engineering blogs 
- Review How to approach a system design and take notes to refer to 
- Work through System design questions with solutions
    - https://excalidraw.com/
- https://www.youtube.com/@jordanhasnolife5163/playlists
- Work through Object-oriented design questions with solutions

- https://github.com/madd86/awesome-system-design if needed

## donnemartin
- [[CAP]] Theorem
    - What is the context of this theorem and what does each three mean?
    - Which one of those is necessary and why?
    - When would you use each of the two combinations?
    - Why can't we achieve all three?
    - What are the three consistency patterns? Why would you use each and why wouldn't you?
    - What are the two availability patterns? 
    - Describe how you would calculate the availbility of two components serving in parallel, each with 99.9% avail
    - Explain what PACELC is
    - TODO review the consistency techniques and see if they fit in the place
    - Case study: How does s3 achieve so many 9s?
    - Case study: How does Google docs handle concurrency?
    - Case study: How do MMO games handle concurrency?
- [[DNS]]
    - Describe a scenario why you would want a latency-based routing
    - Describe a scenario why you would want a geolocation-based routing
    - TODO: attacks
- [[CDN]]
    - What is a CDN and why is it important in modern system design?
    - Explain the caching strategies used by CDNs
    - What are some common challenges in designing a CDN?
    - How does a CDN handle dynamic content differently from static content?
    - How do CDNs ensure consistency and freshness of cached content?
    - What are the trade-offs between global versus regional CDN deployments?
    - How do CDNs handle load balancing and failover?
    - What metrics are used to evaluate CDN performance?
    - How can you integrate a CDN into an existing web architecture?
    - What is the role of DNS in CDN routing?

# Object design
https://python-patterns.guide