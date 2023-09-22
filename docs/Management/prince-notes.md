# Prince 2 - Agile - Notes

Introduction
??? info
    - Prince2 Agile describes ways to configure Prince2 to Agile projects
    - Prince2 and Prince2 Agile are meant for projects only.
        - Projects are temporary and differs from BAU work in significant ways:

            | Projects | BAU Work |
            | -------- | -------- |
            | Temporary | Ongoing |
            | Team created | Stable team |
            | Difficult | Routine |
            | Uncertain | Considerable certainity |
        - It is important to understand the distinction as some of the Agile ways need to be applied differently
        - BAU work:
            - Repeatable, routine tasks performed by a technical team without the need of a Project Manager
            - Usually enhancements are made to an existing product
            - Short timelines and steady flow of work and a dedicated team
        - Project:
            - A temporary situation where a team is assembled to address a problem or opportunity
            - Usually such situations cannot be handled as BAU
            - Projects can be a collection of BAU items
            - Highly uncertain
            - Need to engage a lot of stakeholders
            - It may be geographically spread out and might be part of a larger program
            - Above all it needs to be managed by a Project Manager
        - BAU mode operates on fixed timeboxes.
        - Time boxes are finite period when work is carried out to fulfil an objective
        - Deadlines should not be moved. Timeboxes are used to prioritize the work inside the time duration
        - Timeboxes can be of two types: low level(sprints) or high level(stages)
        ??? note
            - Prince2 treats all agile frameworks as a collection of behaviors, concepts and techniques
            - There are multiple agile frameworks some of which are IT only or are generic.
            - Prince2 treats them as a collection and applies principles on top of it

            | Terms | Examples | Similar Terms |
            | ----- | -------- | ------------- |
            | Behaviors | collaboration, self-organized, trust, blameless | Principles, values, mindset|
            | Concepts | prioritizing what you deliver, reduce waste, inspect and adapt | Fundamentals |
            | Techniques | Burndown chart, user stories, retrospectives | Practices, tools |

Rationale of Prince2 + Agile
??? info
    - Prince2 and Agile tend to complement each other well
    - Prince2 excels in Project Direction and Project Management but weak in project delivery
    - Agile excels in project delivery
    - Thus combining both aspects provide a more holistic approach
    - PRINCE2 Agile provides guidance for tailoring PRINCE2 to work in the most effective way in an agile context.
    - It would be understandable to think that bringing more control and governance into the agile domain could prove counter-productive.
    - However, PRINCE2 Agile represents a marriage that is based on the opposite view: that control and governance allow agile to be used in more situations such as those involving  multiple teams or complex environments.

Prince2 flow using agile
??? info
    - Lifecycle of a PRINCE2 project
    ```mermaid
    graph LR
    A([Pre-Project]) --> B([Initiation])
    B --> C([Subsequent Delivery stages])
    C --> D([Final Delivery stage])
    D --> E([Post-project])
    ```
    - Each of these stages can be represented as Agile processes by considering the following:
        - Plan, monitor & control
        - Behaviors
        - Processes
        - Products

Structure of Prince2
??? info
    - Definition of a project:
        - A temporary organization created to deliver one or more products according to an agreed business case
        - Prince2 should be only be used for projects where the temporary organization needs to be big and complex enough
        - Otherwise setting up process controls can be an overhead
    - Project management cycle:
        ```mermaid
            graph LR
            A[Plan] --> B[Delegate];
            B --> C[Monitor];
            C --> D[Control];
            D --> A;
        ```
    - Definition of a program:
        - A temporary flexible organization created to oversee a set of related projects 
        - Setup to deliver outcomes and benefits in line with strategic objectives
    - Prince2 Structure:
        ```mermaid
            graph TD
            A([Principles]);
            B([Themes]);
            C([Processes]);
            D([Project Environment]);
        ```
    - Principles:
        - Set of guiding principles and best practices that need to be followed to ensure the project is Prince2
        - There are 7 principles:
            - continued business justfication
            - learn from experience
            - defined roles and responsibilities
            - manage by stages
            - manage by exception
            - focus on product
            - tailor to suit projects
    - Themes:
        - Aspects that must be continually addressed parallely
        - 7 themes:
        
        | Theme | Answers | Purpose |
        | ----- | ------- | ------- |
        | Business case | What? | |
        | Organization | Who? | |
        | Quality | What? | |
        | Plans | When? How? How much? | |
        | Risk | What if? | |
        | Change | What is the impact? | |
        | Progress | Where? (are we now and going in future) | |
    - Processes:
        - Project management flow from pre-project to post-project stages
        - Each stage has its own activities, products and responsibilities
    - Project Environment:
        -  Consistent approach of managing projects embedded into orgs way of working
    
