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

    - The INSERT operation:
        - The command inserts one or more rows in a table
        - Ground rules:
            - There is no need to provide all the values for every INSERT operation unless there is a constraint
            - Some columns have defualt or auto generated values. 
            - Auto generated values should not be altered
            - Column values must always match the sequence, data-type and size requirements
            - Numbers should not be provided in quotes. Strings, characters and date-time must be provided in quotes
            - If names of columns are not provided then the values must be provided in a strict sequence
            - Insert can happen only on 1 table at a time
        - Tips:
            - While creating a table in PostgreSQL we can use the `SERIAL` data type to auto increment IDs
            - For date we can use NOW() function in the default values tab to auto add the creation date
            - In case the column name is provided but we need provide the default value we can use the the `DEFAULT` keyword in the values clause
            ```sql
            INSERT INTO department (
                departmentName,
                departmentLoc)
            VALUES
            (
                'Administration',
                DEFAULT
            ),
            (
                'IT',
                DEFAULT
            );
            ```
        - We can create new table which has the some or all the values of another table:
            ```sql
                CREATE TABLE deptdemo AS
                SELECT * FROM departments;
            ```
    - The DELETE Operation:
        ```sql
        DELETE FROM employees
        WHERE empno = 1234;
        ```

    - The ALTER operation:
        - ALTER allows to change one or more properties of a table.
        - It allows making changes to the schemas in the database
        - Example changing the name of a column:
        ```sql
        ALTER TABLE departments RENAME COLUMN dept_name to deptname;
        ```
    - The UPDATE operation:
        - The operation consists of the SET operation and a WHERE CLAUSE
        - the SET clause defines what to do
        - The where clause defines what filters to apply:
        ```sql
        UPDATE product
        SET
	        netretailprice = netretailprice * 0.90
        WHERE
            netretailprice = 24.99;
        ```

Querying Data
??? info
    - The SELECT statement:
        - A typical SQL statement is as follows:
        ```sql
        SELECT [COLUMNS LIST] 
        FROM [TABLE NAME]
        WHERE [CONDITION]
        ORDER BY [COLUMN NAME] [ASC|DESC] 
        LIMIT [N]
        ```
    - The column list can be comma separated like `product_name, quantity`
    - If we want to fetch all columns then use `*`
    - The FROM clause can also be provided as a comma separated list but atleast one is required
    - The WHERE clause provides a way to put conditions on the query
    - The ORDER BY clause provides a way to arrange the output in a specific order
    - LIMIT allows to set a limit on the output
    - Example Query:
        ```sql
        SELECT id, product_id,total, quantity 
        FROM orders
        WHERE quantity > 5
        ORDER BY total desc
        LIMIT 5;
        ```
    - The WHERE clause:
        - The WHERE clause is used to shape data as per conditions
        - There are various conditions that can be used:
            - Comparison:

            | Operation | Sign |
            | --------- | ---- |
            | Finding values between | WHERE <Column name> Between val1 AND val2 |
            | Boolean Operation | >, < , <=, >=, !=, <>|

            - LIKE clause
                - Can be used to specify character patterns
                - Usage: `WHERE <column name> LIKE 'pattern'`
                - Example `WHERE product_name LIKE '_p%'` - This will find all products with name having 2nd letter as p
                - Pattern table for quick reference:

                | Condition | Pattern |
                | --------- | ------- |
                | Any value that ends with letter a | '%a' |
                | Any value that starts with letter a | 'a%' |
                | Any value that has letter a | '%a%' |
                | Any value that starts with b and ends with letter a | 'b%a' |
                | Any value that has a as a second letter | '_a%' |
                | Any value starts with a and has atleast 3 characters | 'a_%_%' | 
    - The JOIN clause
        - Used to fetch data from 2 or more tables together

        | Type | Description | Example | Usage |
        | ---- | ----------- | ------- | --------------- |
        | INNER | Used to find data common to both tables | Finding who ordered from the store | `FROM ORDERS JOIN PEOPLE` |
        | RIGHT | Used to find data from 2nd table and matching data in first | Finding all people who have or have not ordered from the store | `FROM ORDERS RIGHT JOIN PEOPLE` |
        | LEFT | Used to find data from 1st table and matching data in second | Finding all products which may or may not have been ordered | `FROM PRODUCTS LEFT JOIN PEOPLE` |
        | CROSS | Combine data from one column of table 1 and another column of table 2 into 1 column | Find all facecard and value cards | `FROM FACECARD CROSS JOIN VALUECARDS` |
        | UNION | Used to combine 2 queries |  | `<Query 1> UNION <Query 2>` | 

