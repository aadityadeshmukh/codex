# PostgreSQL

??? note "Installation"
    Two ways to install:

    - OS specific installer
        - Just download the OS specific installation and complete recommended setup
    - Containerized approach using docker
        - Just run the below command in the terminal (docker needs to be installed and running):
        ``` terminal
        docker run --name postgre-server -e POSTGRES_PASSWORD=admin -p 5430:5432 -d postgres:latest
        ```

??? note "User Interfaces"
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

??? note "Relational Databases"

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
    Schemas are more like the departments of an organization. It can be used to group together table logically.
    PostgreSQL has a default public schema under which all tables go if no other schema is created.
    - To create a schema just: `Choose create ->Schema` from the right click menu of schemas or databases.
    This will run the following command:
    ```SQL
        CREATE SCHEMA manufacturing
        AUTHORIZATION postgres;
    ```
    - Create 2 schemas *manufacturing* and *human_resources*

    ==Step 3: Tables==
    For now we will add 2 tables to the manufacturing schema called *products* and *categories* with the following columns:

    products:

    - product_id (primary_key)
    - name
    - power
    - manufacturing_cost
    - category_id

    categories:

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
    In order to add link two tables we need to add a foreign key constraint to the table where the foreign key will be referenced.
    For example in our case the the products table will reference the category id so we will apply the foreign key constraint to the product table.

    Using pgAdmin we can apply the Foreign Key constraint by going to table properties -> constraints ->foreign key
    Here specify the tables and columns. Choose validated button and select cascade when updated.

    The sample SQL is as follows:
    ```SQL
    ALTER TABLE IF EXISTS manufacturing.products DROP CONSTRAINT IF EXISTS products_category_id_fkey;

    ALTER TABLE IF EXISTS manufacturing.products
        ADD FOREIGN KEY (category_id)
        REFERENCES manufacturing.categories (category_id) MATCH SIMPLE
        ON UPDATE CASCADE
        ON DELETE NO ACTION;
    ```