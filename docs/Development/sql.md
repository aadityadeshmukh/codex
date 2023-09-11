# SQL - Complete overview

# Basics

Overview of basic SQL commands

??? info
    - The structured query language or SQL comprises of 5 sub languages
    ``` mermaid
    graph TB
    A[SQL - Structured Query Language] --> B[DDL - Data definition language]
    A --> C[DML - Data manipulation language]
    A --> D[DCL - Data control language]
    A --> E[TCL - Transaction control language]
    A --> F[DQL - Data Query language]
    ```
    - DDL: Data definition language
    
        | Command | Description |
        | ------- | ----------- |
        | CREATE  | Creates new db or table  |
        | ALTER   | Modifies the structure of db or table  |
        | TRUNCATE| Removes all table records and allocated table spaces |
        | DROP    | Deletes a db or table |
        | RENAME  | Renames a table or db |

    - DML: Data Manipulation language
        - When you need to change the data itself or perform operations you use DML

        | Command | Description |
        | ------- | ----------- |
        | INSERT  | Adds new row to a table  |
        | UPDATE  | Updates the existing rows  |
        | DELETE | Deletes records from a table |
        | MERGE    | Also called UPSERT add or update data based on conditions |
    
    - DCL - Data Control language
        - Control = Authorization.
        - For authorization you either grant or revoke access
        
        | Command | Description |
        | ------- | ----------- |
        | GRANT  | Gives access previliges to user  |
        | REVOKE  | Removes access previliges  |
    
    - TCL - Transaction control language
        - Data manipulation happens outside of the container and added to container via transactions

        | Command | Description |
        | ------- | ----------- |
        | COMMIT  | Saves changes to database permanently  |
        | ROLLBACK  | Restores the database to original form until last commit  |
        | SAVEPOINT | Creates point for later use for rollback |
        | SET TRANSACTION    | Set transaction properties to make it read-only |
    - DQL - Data Query language
        - Single keyword in this criteria: `SELECT`
        - Retrieves information from the database based on parameters
    
    - ==Data Types==
        - Typical data types are fo 5 main types:
            - Numeric: INT, BIGINT, FLOAT, REAL
            - String: CHAR, VARCHAR
            - Binary: BINARY, VARBINARY
            - Miscellaneous: DATE, TIME, DATETIME
            - Proprietary: Unique to the DB system. For example MSSQL has MONEY as a type

    - Basic Queries:
        - To insert values in a table:
            ```sql
            INSERT INTO public.product(
                productid, categoryid, supplierid, productname, netretailprice, availableqty, wholesaleprice, unitkgweight, notes)
                VALUES (1, 5, 2, 'Calculator' ,24.99 ,100 ,17.99 ,1, 'app'),
                (2, 5, 5, 'Penwrite' ,79.99 , 27, 49.99, 2, 'device'),
                (3, 1, 6, 'Vortex generator' ,2499.99 , 1000, 1999.99, 0.01, 'space engine'),
                (4, 1, 6, 'Gourmet crockpot' ,24.99 , 72, 19.99, 1.63, 'utensil');
            ```
        - To get the data from a table for all columns: `SELECT * FROM public.product;`

Manipulating Data
??? info
    
        