---
hide:
    - navigation
---
# Devops Fundamentals


- DevOps stands for Development & Operations. 
- It means shared responsibilities between the development and operations teams.
- The development team work being aware of the challenges faced by operations and contribute and ops team work more like developers with proper flow and process.

Basics

??? info
    - Values:
        - C (Culture) - How people interact
        - A (Automation) - Automation at the heart of solution to the problem
        - M (Measurement) - What to measure and incentivize accross the organization
        - S (Sharing) - Feedback loops for discrete regular improvements based on transparency
    - Principles:
        - Systems Thinking
            - Consider the outcome of the entire pipeline or value chain
            - For example adding app servers might overload the db with connections
            - In case of IT the process might be helpful for the sub org but making the delivery slow
            - Systems thinking must be used as guidance to set proper success criteria and evaluation of the system 
        - Amplify Feedback loops
            - Effective feedback is what drives any control loop designed to improve the process
            - Use amplify feeback loops to help when creating multi-team processes, visualizing metrics & designing delivery flows
        - Continuous experimentation and learning
            - Focus on doing rather than talking about it
            - Team must be ready to learn new things and the best way is to try to see if it works
            - Use the approach to define team processes and standards, and as part of the leadership style
    - Playbook (set of methodologies):
        - People over process over tools
            - Choose people, then define process and then choose the tools and not the other way around
        - Lean management
            - Work in small batches
            - Work within progress limits
            - Feedback loops
            - Visualization
        - Continuous delivery
            - Code and release code regularly and in small batches
        - Visible Ops-style change control:
            - Eliminate fragile artifacts
            - Create repeatable build process
            - Manage dependencies
            - Create and env of continuous improvement
        - Infrastructure as Code
            - System can and should be treated as code
            - This standardizes the infrastructure and reduces the effort
    - Practices:
        - Incident management system
        - Devs on call
        - Public status pages
        - Blameless postmortems
        - Embedded teams
        - The cloud
        - Andon cord
        - Dependency Injection
        - Blue green deployment
        - Chaos monkey
    - Tools must be:
        - Programmable
        - Verifyable
        - Well behaved with the other parts of the system

DevOps: Culture problem

??? info
    - When development and operations start working in silos problems begin to arise
    - Problem is both groups are incentivised differently
    - Both groups optimise their flows but it creates a less efficient overall system
    - This needs to be solved by a culture shift
    - Ways to do it:
        - Communication (Blameless postmortems):
            - Have a postmortem within 24-48 hours of the outage
            - Build a timeline of the events
            - Analyse the issues and discuss possible solutions
            - Discuss how the customers were affected
            - Document the learnings
            - Discuss how can detection be done earlier in similar cases in the future
            - Optimize for failure and recover than just prevention
        - Communication (Transparent uptime):
            - Admit failure
            - Have an open communication channel
            - Be authentic
            - Have a POC
        - Collaboration:
            - Have a team that has people working on both dev and ops aspects
            - Practice openness:
                - Open chatrooms
                - Open wiki pages etc.
        - Management best practices:
            - Cross functional teams
            - Help people through the change
            - Use Lean Agile processes
        - Kaizen (Continuous Improvement):
            - Principles:
                - Good process bring good results
                - Go see for yourself
                - Speak with data manage by facts
                - Take action to correct and contain root causes
                - Work as a team
                - Its everyone's business
            - 5 Whys
                - Ask questions in repeated iterations
                - Do not accept time constraints as an answer find out what lead to the delay
                - Do not accept manual failure as answer find out what process failed
    
Building blocks of DevOps
??? info
    - The main building blocks of DevOps are:
    - Agile
        - DevOps is deep rooted in Agile
        - Its highky suggested DevOps be implemented in conjunction with Agile as they are highly complimentary
        - DevOps has roots in Agile and the process are iterative which generates quick product or solution delivery.
    - Lean
        - Principles:
            - Eliminate waste
            - Amplify learning
            - Decide as late as possible
            - Decide as fast as possible
            - Empower the team
            - Build integrity
            - See the whole
        - Techniques:
            - Kaizen
            - Value-Stream mapping
    - ITIL, ITSM and SDLC
        - These are prescriptive models mostly predecessors of modern day DevOps

Infrastructure as Code
??? info
    - Infrastrucure as Code is a complete programmatic approach to infrastructure management
    - It allows to manage infrastructure with the same principles as software development
    - With IaC we can code the scripts in an IDE, run tests, apply decision making based on state and deploy automatically
    - For example, we can completely describe an AWS system as code using a format called cloud formation which enables to replicate the system all the time
    - Configuration Management:
        - Concepts:
            - Provisioning: Process of making a server ready for operation using hardware, OS, system drivers & network connectivity
            - Deployment: Automatically deploying and upgrading applications on a server
            - Orchestration: Co-ordinating operations within multiple systems
            - Configuration management: Overarching term for management of change control for system configuration after initial provision
        - Techniques - how tools approach configuration management
            - Imperative / procedural: Commands necessary to produce a state and defined and executed
            - Functional / Declarative: We define the state and the tool converges the exisiting configuration based on the desired state
            - Idempotent: Repeat execution equals same exact model
            - Self-service: No need for manual intervention other than the requesting user
    - Common toolchain:
        - For AWS: provisioning can be done via AWS cloud formation
        - For Azure: Azure resource manager
        - Terraform: Allows to provision in a more abstract way which can be translated to multiple platforms

Continuous Integration\Delivery
??? info
    - Continuous Integration: Automatically building and unit testing the entire application at regular intervals ideally at each code check-in
    - Continuous Delivery: Automatically deploying every change to a production like environment and performing integration and acceptance testing
    - Continuous Deployment: After automated testing automatically deploying the change to production
    - Advantages:
        - Decreased time to market
        - Quality increase
        - Go live is not an event
        - Lead time for changes is reduced
        - State of Devops: having short lived feature branches and having less than 3 overall branches improves efficiency
        - Lower mean time to recover
    - CI practices:
        - Short build times. Coffee test
        - Commit really small bits
        - Don't leave the build broken
        - Trunk based development flow
        - Don't allow flaky tests
        - The build must return a status, log and artifact marked with the build number
    - CD practices:
        - No separate artifact for different environments
        - Artifacts should be immutable
        - Staging should be a copy of production
        - Stop deployes if a previous step fails - (Andon cord)
        - Deployments should be idempotent

Reliability Engineering:
??? info
    - Ability of a system or component to function for a specified period of time
    - MTTR: Mean time to recovery
    - MTBF: Mean time between failures
    - Reliability engineering typically involves embedding product knowledge into operations and operational knowledge into product
    - Design for Operation:
        - Use design patterns (Gang of 4)
        - Use reliability patterns like circuit breaker (Netflix, Hysterix library)
        - [12 factor app](https://12factor.net/)
        - The success of the overall app relies on using the right patterns at the very beginning of the toolchain
        - Have operational intelligence with the development capabilities
        - This will provide better code shipped which is resilient and performant
        - Netflix's chaos monkey actively kills servers and developers need to factor in this when they create applications
    - Operate for design:
        - Use a lean approach to monitoring and metrics
        - Build a MVP, target a few systems, learn, repeat and go deeper as needed
        - Build just enough metrics to gain insights and not overload the systems
        - Areas for monitoring service uptime, application uptime, security, system usage, etc.
        - Have a minimal centralized logging mechanism
        - SRE toolchain:
            - Monitoring: Grafana, Containers (Prometheus)
            - Logging: Splunk, ELK stack (Elasticsearch, logstash, kibana)
            - Statuspage.io provides status pages as a service
            - Security - Checkmarx (FOSS scans)

