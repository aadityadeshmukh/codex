---
hide:
    - navigation
    - toc
---

# Power BI

## Intro to BI

- Business Intelligence or BI is using data to make better decisions
- Better decisions to take the business forward
- Examples:
    - ways to attract new or retain old customers
    - competitor analysis
    - what is driving profits?
    - what expenses can be diminished?
- Key concepts:
    - Domain
        - The context in which BI is applied
        - Ex. std business functions or depts like sales, marketing etc.
        - Helps narrow focus
    - Data
        - Next step after selecting a domain is selection of relevant data
        - That means finding sources of relevant data:
            - Internal Data
                - Data generated within the boundaries of an org
                - net revenues, units produced, sales etc.
            - External Data
                - BI is most effective when internal data is combined with external data
                - data created outside the boundaries of an org
                - Eg. global economic performance, census info, competitor prices etc.
        - Also types of data:
            - Structured
                - Data that conforms to a specific structure with rows and columns
                - Mostly relational database systems and database standards (ODBC, OLE DB) fall in this category
            - Unstructured
                - Data that cannot be organized into standard rows and columns
                - Videos, audio, images, text
                - word, pdfs, emails, social media posts
                - these are most difficult to consume and analyze
                - stored as BLOBS (binary large objects) or as a file in file systems (NTFS, HDFS)
                - No-SQL databases also fall under this category
            - Semi-structured
                - Structured data but not conforming to row/column standard
                - Delimited text files, XML, markup languages, JSON, EDI
                - They have self-defining structure which makes them easy to consume and analyze but harder than true structured data
                - data access protocols like OData / REST also fall in this category
        - BI tools are optimized for handling structured and semi-structured data
        - BI tools are designed to ingest semi-structured data sources and transform them to structured
    - Model
        - A data model refers to the way in which one or more data sources are organized
        - The organization is done to support analysis and visualization
        - Key steps:
            - Organizing
                - Organzing is done by establishing how the multiple data sources relate to one another
                - This helps model become a cohesive whole
                - For example the relation between a CRM data and ERP data can be done based on customer name
            - Transforming and cleansing
                - It is almost always necessary to clean the source data
                - Duplicates, trailing spaces, spelling mistakes, missing data
                - Transforming and cleansing tech are often called ETL and have special tools
            - Defining and cetegorizing
                - Defining data types on the source data eg. date, time, datetime etc.
                - Defining catergories in each data type for example a text can be a url
                - These provide ways to establish what analysis can be done on each type
    - Analysis
        - Grouping data, simple aggregations, sums, counts and averages
        - sPecialized calculations to identify trends, correlations and forecasting
        - Creating KPIs
        - There are tons of tools to do the analysis. R, Python and SQL are top ones along with Excel.
    - Visualization
        - Visualizations are tables, matrices, pie charts, bar graphs, and other visual displays that help provide context and meaning to the analysis
        - Business intelligence tools allow multiple individual tables and charts to be combined on a single page or report.

## Power BI ecosystem

- Power BI is a collection of interralted tools and services that form a complete BI ecosystem.
- It includes tools for modelling, analysis and visualization
- The ecosystem can be broken down into following categories:
    - Core - Power BI specific
        - Power BI Desktop:
            - Free, Windows based app that installs on a local computer
            - Its primary tool to ingest, cleanse, transform data, combine into models
            - Analyze and visualize using calculations, visualizations, and reports
            - Once reports are created in Power BI desktop they are published to Power BI service
        - Power BI service:
            - Cloud-based SaaS online platform
            - light report editing, sharing, collaborating, and viewing reports
    - Core - non Power BI specific
        - Data Query - Data connectivity and transformation
        - DAX - programming language for Power BI
        - On Premise data gateway - facilitate access from Power BI service to data sources
        - SSAS - Ability to build models
        - MS Appsource - Marketplace to find apps, add-ins and extensions to sotwares like Power BI
    - Non-core - Power BI specific
        - Report Server
        - Embedded
        - Mobile application
        - Mixed reality
    - Natively integrated MS tech
        - Office 365, Excel, Microsoft Flow, Power Apps, Visio, Azure ML, Report builder
    - Extended ecosystem
        - Large ecosystem of 3rd party tools and add-ons

## Power BI Desktop

## Connecting and Shaping Data

## Data Models and Calculations

## Unlocking Insights

## Final Report

## Publishing and Sharing

## Using reports in the service

## Dashboard, Apps & security

## Gateways and refreshing Datasets