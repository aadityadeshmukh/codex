# Introduction to Snowflake DB

## Overview

- Snowflake DB is a cloud-native SaaS data platform
- It provides data platform services with 0 install requirement
- It connects to all major cloud providers
- Uses columnar storage and follows a STAR schema
- Provides ways to aggregate data as well (average, sum, min, max etc.)

#### Snowflake Data platform

![Snowflake basics](../assets/images/Technologies/snowflake-basic.png)

![Snowflake Architecture](../assets/images/Technologies/snowflake-arch.png)

#### Date warehouse vs Data lake

| Data Warehouse | Data Lake |
| -------------- | --------- |
| Structured Data | Unstructured Data (files) |
| Tables | Stored as objects |
| Schema on read | Schema on read |
| Huge Volume | Massive Volume |

- Data Lake house is a mix of these concepts

#### Snowflake - Data warehousing analytics solution

| Used for | Not Used for |
| -------- | ------------ |
| Ad-hoc reads | OLTP |
| Data ware house reads | Transactions |
| Data lake reads | Constraint enforcement |

- Key features:
    - Virtual, multi-cluster warehouse
    - Time-travel and zero copy cloning
    - Data sharing and marketplace
    - High availability - cross cloud replication
    - Granular security and automatic encryption
    - Strong support for JSON

#### Snowflake tools

The tools used to interact and use Snowflake:
    - Web UI and console
    - SnowSQL - Python based CLI
    - Snowpipe - Streaming based ingest
    - Drivers and SDK for other languages
    - Partner connections

## Snowflake Storage

- Snowflake storage is:
    - On cloud blob storage or file storage
    - Scalable and available
    - Encrypted
    - Compressed
    - Auto-partitioned

- Ways to load data:
    - External Table
    - Copy command
        - Large single shot copies
    - SnowPipe object
        - Micro copies. continuous entries

- Data load process:

```mermaid
graph LR
A([Source from bucket]) --> B([Create file format]);
B --> | <= 100MB|C([Load from WebUI]);
B --> |PUT command| D([CLI]);
C --> E([Staging Table]);
D --> E;
E --> |Default|F[Permanent Storage]
E --> G[Temporary Storage]
E --> H[Transient Storage]
E --> |Data Lakes|I[External]
```




