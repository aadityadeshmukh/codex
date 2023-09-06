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
        - S (Sharing) - Deedback loops for discrete regular improvements based on transparency
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