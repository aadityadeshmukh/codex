---
tags:
    - python
---
# Python Database API

**Introduction**
??? info
    - There are multiple database systems out there. 
    - Even considering relational databases there are multiple implementations.
    - Interacting with these databases can be tedius since the implementations are different.
    - Python provides a neat standardized way to interact with these databases
    - Its called the Python Database API
    - It provides ways to connect to the database (connection API) and performing operations (cursor API) from python itself

**Creating a database in PostgreSQL**
??? info
    - Short answer? Go to pgAdmin and create a new database from the UI.
    - For more details please refer to the [PostgreSQL Quick start](postgre.md)

**Creating a table in PostgreSQL using python**
??? info
    - For this we need a module called *psycopg2*
    - The process is simple:
        - Open a connection to the database
        - Open a cursor using the connection object
        - Execute the command
        - Commit the command
        - Close the connection
    - Install the psycopg2 module using: `pip install psycopg2-binary`
    - Example code:
        ```python
        import psycopg2

        conn = psycopg2.connect(
            database="red30",
            user="postgres",
            password="admin",
            host="localhost",
            port="5430")

        cursor = conn.cursor()

        cursor.execute('''CREATE TABLE sales(
            order_num INT PRIMARY KEY,
            cust_name TEXT,
            product_num TEXT,
            product_name TEXT,
            quantity INT,
            price REAL,
            discount REAL,
            order_total REAL);''')

        conn.commit()
        conn.close()
        ```


**Inserting data in PostgreSQL using Python**

??? info
    - The process remains the same.
    - Open connection
    - Get the cursor
    - Execute commands:
        ```python
            # open connection
            # get cursor

            sales = [(1,"John Doe","OBJ1","ITE1",2,89,0,178),
            (2,"Jane Doe","OBJ2","ITE3",1,44.95,0,44.95),
            (3,"John Doekar","OBJ7","ITE7",3,399,0.1,1077.3)]

            cursor.executemany("INSERT INTO sales VALUES(%s, %s, %s, %s, %s, %s, %s, %s)", sales)

            # commit changes
            # close connection
        ```


**Interacting with PostgreSQL database using Python**

??? info
    - A typical program to insert values based on some logic running in python is shown below.
    - Here the function insert_sale takes input taken as arguments, calculates order total and inserts values:
    ```python
    def insert_sale(order_num, customer_name, product_num, product_name, quantity, price, discount, order_total):
    order_total = quantity * price
    if discount!=0:
        order_total -= discount
    #create a dictionary
    sale_data = {
        'order_num': order_num,
        'cust_name': customer_name,
        'product_num': product_num,
        'product_name': product_name,
        'quantity': quantity,
        'price': price,
        'discount': discount,
        'order_total': order_total}
    
    cursor.execute("INSERT INTO sales VALUES(%(order_num)s, %(cust_name)s, %(product_num)s, %(product_name)s, %(quantity)s, %(price)s, %(discount)s, %(order_total)s)", sale_data)
    ```


**SQLAlchemy Core to connect \ manipulate to a Postgres database**

??? info
    - Install SQL Alchemy core using `pip install sqlalchemy`
    - To connect to a database table use the below code:
    ```python
    from sqlalchemy import create_engine, select
    from sqlalchemy import Table, Column, Integer, Float, String, MetaData

    # Create the engine object using create_engine function
    engine = create_engine('postgresql+psycopg2://postgres:admin@localhost:5430/red30', 
	echo=True)

    # Create the metadata object
    metadata = MetaData()

    # Build the table structure as a list if need to create using Table object
    
    # sales_table = Table('sales', 
    #       metadata,  
    #       Column('order_num', Integer, primary_key=true),
    #       Column('cust_name', String),
    #       Column('prod_number', String),
    #       Column('prod_name', String),
    #       Column('quantity', Float),
    #       Column('price', Float),
    #       Column('discount', Float),
    #       Column('order_total', Float))

    # Connect to the table with the Table object

    sales_table = Table('sales', metadata, autoload_with=engine)

    # pass engine object to the metadata create_all function
    metadata.create_all(engine)

    with engine.connect() as conn:
        #Read
    for row in conn.execute(select(sales_table)):
        print(row)

        # Create
        insert_statement = sales_table.insert().values(order_num=1105910, 
            cust_name='Syman Mapstone', 
            product_num='EB521', 
            product_name='Understanding Artificial Intelligence', 
            quantity=3,
            price=19.5, 
            discount=0, 
            order_total=58.5)
        conn.execute(insert_statement)

        # Update
        update_statement = sales_table.update().where(sales_table.c.order_num==1105910).values(quantity=2, order_total=39)
        conn.execute(update_statement)

        # Confirm Update
        reselect_statement = sales_table.select().where(sales_table.c.order_num==1105910)
        updated_sale = conn.execute(reselect_statement).first()
        print(updated_sale)

        # Delete
        delete_statement = sales_table.delete().where(sales_table.c.order_num==1105910)
        conn.execute(delete_statement)

        # Confirm Delete
        not_found_set = conn.execute(reselect_statement)
        print(not_found_set.rowcount)
    ```


**SQLAlchemy ORM to connect \ manipulate to a Postgres database**

??? info
    - Core is a function based approach to connect while ORM is class based approach.
    - Sample crud operations along with connections etc are shown below:
        ```python
        from sqlalchemy import create_engine, select
        from sqlalchemy.orm import Session
        from sqlalchemy.ext.automap import automap_base

        engine = create_engine('postgresql://postgres:password@localhost/red30')

        # class Sale(Base):
        #       __tablename__='sales',
        #       Column('order_num', Integer, primary_key=true),
        #       Column('cust_name', String),
        #       Column('prod_number', String),
        #       Column('prod_name', String),
        #       Column('quantity', Float),
        #       Column('price', Float),
        #       Column('discount', Float),
        #       Column('order_total', Float))

        Base = automap_base()

        Base.prepare(autoload_with=engine)

        Sales = Base.classes.sales

        with Session(engine) as session:

            # Read
            smallest_sale = session.execute(select(Sales).order_by(Sales.order_total)).scalar()
            print(smallest_sale.order_total)

            # Insert
            recent_sale = Sales(order_num=1105910, cust_name='Syman Mapstone', prod_number='EB521', prod_name='Understanding Artificial Intelligence', quantity=3, price=19.5, discount=0, order_total=58.5)
            session.add(recent_sale)
            session.commit()

            # Update
            recent_sale.quantity = 2
            recent_sale.order_total = 39
            updated_sale = session.execute(select(Sales).filter(Sales.order_num == 1105910)).scalar()
            print(updated_sale.quantity)
            print(updated_sale.order_total)
            session.commit()

            # Delete
            returned_sale = session.execute(select(Sales).filter(Sales.order_num == 1105910)).scalar()
            session.delete(returned_sale)
            session.commit()
        ```
