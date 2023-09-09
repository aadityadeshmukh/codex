---
hide:
    - navigation
---
# Continuous Integration / Continuous Delivery

Benefits of CI/CD:
??? info
    - CI/CD systems can differ a lot depending on the technology
    - However, there are 5 key benefits one can see from CI/CD:
        - Empowering teams: The pipeline is a self service system. This makes the system transparent and easy to use by anyone
        - Cycle Time: The cycle time from code commit to production is reduced significantly
        - Security issues: Reduces security issues as security compliance integrates each time
        - Continuous practice: Release date is not a big event
        - More value: Spend less time doing unplanned work and more time value added work
    - A typical CI/CD pipeline in practice:
        ``` mermaid
            graph LR
            A[Source Code] --> B[Version Control];
            B -->C[Build Tool];
            C --> D[Build & tests];
            D --> G[Artifact store];
            G --> H[Local Deployment]
            H --> I[Deployed and tested]
            I --> J{Testing successful?}
            J --> |YES| K[Production Deployment]
            J --> |NO| L[Go to start of cycle]
        ```
