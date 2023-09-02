# PostgreSQL Quick Start

??? info "Installation"
    Two ways to install:

    - OS specific installer
        - Just download the OS specific installation and complete recommended setup
    - Containerized approach using docker
        - Just run the below command in the terminal (docker needs to be installed and running):
        ``` terminal
        docker run --name postgre-server -e POSTGRES_PASSWORD=admin -p 5430:5432 -d postgres:latest
        ```

??? info "User Interfaces"
    ==PSQL Shell==

    PSQL shell is a command line interface to work with the Postgre databases.

    Quick commands:

    - `\l` list od databases
    - `\c <database name>` change context to database
    - `SELECT version();` : shows the version
    - `SELECT now();`: shows server time
    - `CREATE DATABASE colors;` : create new database
    - `CREATE TABLE colors (ColorID int, ColorName char(20));` : creating a new table
    - `INSERT INTO colors VALUES (1, 'red') , (2, 'blue'), (3, 'green');`: inserting values
    - `SELECT * FROM colors;`: fetching data

    ==PGAdmin==

    PGAdmin is a graphical user interface to connect to the PostgreSQL servers.
    Each server has 3 parts:

    - Databases
    - Login/Group roles
    - Tablespaces

    - Tables are located in Databases->Db Name->Schema->Tables->Table Name
    - The client has a query tool to fetch data from the tables
    - To quickly get all rows: From table right click menu select "View/Edit Data-> All rows"
    - To connect the PG Admin to postgres on docker just enter the login details and server details

??? info "Relational Databases"

    Database table structure:

    - Each table has a rows and columns
    - the first row is usually the primary key. convention: `tablename_id`
    - other columns might represent different types of data

    Common Datatypes:

    - Numeric:
        - Whole numbers: `int``, `bigint`, `smallint` data types
        - fractions: `numeric`, `decimal` data types. both will allow significant digit specification
        - floating points: `real` or `double` precision
    - Characters
        - Use `character(n)` or `char(n)`: for fixed length 
        - Use `varchar(n)` for variable length
        - Use `text` for unlimited text with no cap
    - Date & Time
        - `date`
        - `time`
        - `timestamp`
        - `timestamp with timezones`

??? example "Sample database from scratch"

    For our sample we are going to consider a company called *"Kinetico"*. 
    We will build the organizational structure in PostgreSQL using all of its main features.

    ==Step 1: Create Database==
    
    In pgAdmin right click on databases->Create->database.
    This in turn will run the following command in the background:
    ```SQL
        CREATE DATABASE "kinetico"
        WITH
        OWNER = postgres
        ENCODING = 'UTF8'
        CONNECTION LIMIT = -1
        IS_TEMPLATE = False;
    ```

    ==Step 2: Schemas==
    
    - Schemas are more like the departments of an organization. 
    - It can be used to group together table logically.
    - PostgreSQL has a default public schema under which all tables go if no other schema is created.
    - To create a schema just: `Choose create ->Schema` from the right click menu of schemas or databases.
    - This will run the following command:
    ```SQL
        CREATE SCHEMA manufacturing
        AUTHORIZATION postgres;
    ```
    - Create 2 schemas *manufacturing* and *human_resources*

    ==Step 3: Tables==
    
    - For now we will add 2 tables to the manufacturing schema called *products* and *categories* with the following columns:

    - products:
        - product_id (primary_key)
        - name
        - power
        - manufacturing_cost
        - category_id

    - categories:
        - category_id
        - name
        - market

    ```SQL
    CREATE TABLE manufacturing.products
    (
        product_id character varying(10) NOT NULL,
        name character varying(100) NOT NULL,
        power integer,
        manufacturing_cost numeric(10, 2) NOT NULL,
        category_id integer NOT NULL,
        PRIMARY KEY (product_id)
    );

    ALTER TABLE IF EXISTS manufacturing.products
        OWNER to postgres;
    ```
    
    ==Step 4: Link tables with Foreign Keys==
    
    - In order to add link two tables we need to add a foreign key constraint to the table where the foreign key will be referenced.
    - For example in our case the the products table will reference the category id so we will apply the foreign key constraint to the product table.

    - Using pgAdmin we can apply the Foreign Key constraint by going to table properties -> constraints ->foreign key
    - Here specify the tables and columns. Choose validated button and select cascade when updated.

    The sample SQL is as follows:
    ```SQL
    ALTER TABLE IF EXISTS manufacturing.products DROP CONSTRAINT IF EXISTS products_category_id_fkey;

    ALTER TABLE IF EXISTS manufacturing.products
        ADD FOREIGN KEY (category_id)
        REFERENCES manufacturing.categories (category_id) MATCH SIMPLE
        ON UPDATE CASCADE
        ON DELETE NO ACTION;
    ```

    ==Step 5: Add data to tables with CSV==

??? info "Querying Data"

    - Basic clauses:
        - SELECT : Allows to specify the columns needed to be query. If all needed use * as a shortcut
        - FROM : Allows to specify the schema.table from where the query needs to be done
        - WHERE : Allows to fetch only those rows which meet a criteria

        ```SQL
        SELECT *
        FROM manufacturing.products;
        ```

        ```SQL
        SELECT name, manufacturing_cost
        FROM manufacturing.products;
        WHERE product_id = 'KE9W';
        ```
    
    - Joining two tables to query data:
        - SELECT: Use tablename.columnName comma separated values and use alias clause (AS) to specify ambiguous values
        - FROM: Use `schemaName.tableName JOIN schemaName.tableName`
        - ON: Use to specify the categories on which joins will happen
        - WHERE: Filter the data
    
        ```SQL
        SELECT products.product_id,
            products.name AS product_name,
            products.manufacturing_cost,
            products.category_id,
            categories.name AS category_name,
            categories.market
        FROM manufacturing.products JOIN manufacturing.categories
        ON products.category_id = categories.category_id
        WHERE categories.name = 'batteries';
        ```
    
    - Creating a view to easily query joined data:
        - In case the joined data needs to be queried multiple times then views can be created.
        - Process is to write the query and add the following syntax before it: `CREATE VIEW tableName.viewName AS`
        - A view will be created which can be used a separate table
        - A view acts like a table but does not duplicate data

    ```SQL
    CREATE VIEW manufacturing.product_details AS
    SELECT products.product_id,
        products.name AS product_name,
        products.manufacturing_cost,
        products.category_id,
        categories.name AS category_name,
        categories.market
    FROM manufacturing.products JOIN manufacturing.categories
    ON products.category_id = categories.category_id;
    ```

??? info "Indexing & constraining data"
    - Index is a typical like a collection of keywords you find at the end of a text book to quickly find concepts.
    - You can use it to find information frequently needed by creating an index on a table.
    - To create an index open the table needed and from the right click menu choose create
    - There are a few options in the creation panel like Unique, Clustered etc.
    
    - Constraints are conditions we add to the database so that only the correct data is added
    - Click on Constraints->Create and add a name and condition(s) to create a constraint

??? danger "Administration"

    - Postgre provides some administrative capabilitites. 
    - We can use roles to provide unlimited or restricted access to the users.
    - Some common syntax is mentioned below to:
        - Set role
        - Reset role
        - Grant permissions
        - Revoke role
        - drop the role
    
    ```SQL
    -- View tables from the KinetEco database
    SELECT * FROM manufacturing.products;
    SELECT * FROM human_resources.employees;

    -- Impersonate the hr_manager
    SET ROLE hr_manager;

    -- Switch permissions back to posgres super user
    RESET ROLE;

    -- Give hr_manager permissions in database
    GRANT USAGE ON SCHEMA human_resources TO hr_manager;
    GRANT SELECT ON ALL TABLES IN SCHEMA human_resources TO hr_manager;
    GRANT ALL ON ALL TABLES IN SCHEMA human_resources TO hr_manager;

    -- Remove the hr_manager role from Postgres Server
    RESET ROLE;
    REVOKE ALL ON ALL TABLES IN SCHEMA human_resources FROM hr_manager;
    REVOKE USAGE ON SCHEMA human_resources FROM hr_manager;
    DROP ROLE hr_manager;
    ```