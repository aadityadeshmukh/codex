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

Sample CI/CD pipeline
??? info
    - Version Control
        - Standard tool: Git
        - Version Control best practices:
            - Always use version control
            - Put everything in version control
            - Commit often. small batches
            - Succint Commit messages
            - do not commit code whose build is failing
            - Consider using master branch approach (TBD)
            - precommit hooks for quality control
    - Continuous Integration
        - Defacto app: Jenkins
        - Best practices:
            - Start with a clean build
            - Build to pass the coffee test
            - Run tests locally before committing
            - Do not commit to a build which is broken
            - Do not leave the build broken
            - Do not remove failing tests. remove them if they are not valid
            - Use notifications to communicate build status
            - Datadog can be used to co-relate errors to code changes
    - Artifact Repositories
        - Examples: Nexus or Artifactory
        - Benefits of packaging code as artifacts:
            - Composeability
            - Reliability
            - Security
            - Shareability
    - Testing & Continuous Delivery:
        - Unit Testing: Run with builds. Eg. JUnit
        - Integration Testing: Run to check integration of various components. Eg. ServerSpec
        - E2E testing: Run to check from a user flow perspective. Eg. Selenium tests
        - Test philosophy:
            - TDD
            - BDD : Cucumber
            - ATDD : Acceptance Test driven development
        - Application Deployment and release:
            - Deploy with same artifact, in the same way, in an identical environment
            - Tool: Ansible
