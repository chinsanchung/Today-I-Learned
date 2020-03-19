# # Nanodgree 2. SQL and Data Modeling for the Web

## chapter 03. SQLAlchemy Basics

### 1. Introduction

#### Takeaway

- SQLAlchemy
    - SQLAlchemy is the most popular open-source library for working with relational databases from Python.
    - It's one type of **ORM** libray (**Object-Relational Mapping**), which provides an interface for using object oriented programming to interact with a database.
    - Features **functional-based query construction**: allows SQL clauses to be built via Python functions and expressions.
    - **Avoid writing raw SQL.** It generates SQL and Python code for you to access tables, which leads to less database-related overhead in terms of the volume of code you need to write overall to interact with your models.
    - Moreover, you can **avoid sending SQL to the database on every cal**. The SQLAlchemy ORM library features **automatic caching**, caching collections and references between objects once initially loaded.
- ORM
    - Maps tables and columns to class objects and attributes.
    - It allows you to interact with database using an object-oriented language like Python, rather than writing raw SQL.
- Reason to use SQLALchemy over writing raw SQL
    - Write less bug-prone code
    - Work entirely in Python
    - Be able to switch database systems easily without rewriting code

### 2. Layers of Abstraction

- layer of abstraction: they are built into the library for offering more convenient methods of database interaction.
    - We can choose many different ways of interacting with the database, because it offers us multiple layers of abstractions for doing so.

#### Takeaways

- Without SQLAlchemy, we'd only use a DBAPI to establish connections and execute SQL statements.
- SQLAlchemy offers several layers of abstraction and conveinent tools for interacting with a database.
- SQLAlchemy lets you 
    - Can stay on the ORM level.
    - Can dive into database operations to run customized SQL
    - Can write raw SQL to execute. (Can more simply use `psycopg2` in this case)

#### SQLAlchemy vs psycopg2

- SQLAlchemy **generates** SQL statements.
- psycopg2 directly **sends** SQL statements to the database.
- SQLAlchemy depends on psycopg2 or other database drivers to communicate with the database, under the hood.

#### Good Design Practice

- Keep your code Pythonic. Work in classes and objects as much as possible.
- Avoid writing raw SQL until absolutely necessary.

#### Layers of SQLAlchemy

1. DBAPI
2. The Dialect
3. The Connectional Pool
4. The Engine
5. SQL Expressions
6. SQL Alchemy ORM (optional)

### 3. The Dialect

- Because of dialect, we can forget about the database system we're using.

### 4. The Connection Pool

- Because SQLAlchemy has a connection pool, no longer do you have to connect to open and close your own connections manually using the DPAPI alone.
- With a Connection Pool, the opening and closing of connections and which connection you are using when you're executing statements within a session is completely abstracted away from you.
- Uses a connection pool to easily reuse existing connections.
    - Avoid opening and closing connections for every data change.
    - Handles dropped connections.
    - Avoid doing very many small calls to the DB when we're continually assigning changes to the database. (which can be very slow)

### 5. The Engine

#### Takeaways

- The Engine
    - one of three main layers for how you may choose to interact with the database.
    - It is the lowest level layer of interacting with the database, and is much like using the DBAPI directly.
        - It looks like interacting with a DBAPI directly. (very similar to using psycopg2)
```python
from sqlalchemy import create_engine
engine = create_engine('postgres;//...')
conn = engine.connect()
result = conn.execute('SELECT * from vechicles;')

row = result.fetchone()
rows = result.fetchall()
result.close()
```
- The Engine in SQLAlchemy refers to both itself, the Dialect and the Connection Pool, which all work together to interface with our database.
- A connection pool gets automatically created when we create SQLAlchemy engine.

### 6. SQL Expressions

- SQL Expression lets you compose SQL statements by building Pyhton objects instead.

```python
todos = Table('todos', ...)
ins = todos.insert().values(dscription='Clean my room', completed=False)
s = select([todos])
conn = engine.connect()
result = conn.execute(ins)
result = conn.execute(s)

result.close()
todos.c.desciption # <Column 'description' in `todos` table>
```

#### Takeawayas

- Instead of sending raw SQL (using the Engine), we can compose python objects to compose SQL expressions, instead.
- SQL Expressions still involves using and knowing SQL to interact with the database.

### 7. SQLAlchemy ORM

#### Takeaways
- ORM
    - SQLAlchemy ORM lets you compose SQL Expressions by building Python classes of objects
    - It is the highest layer of abstraction in SQLAlchemy.
    - Wraps the SQL Expressions and Engine to work together to interact with the database.
- SQLAlchemy is plit into two libraries
    - SQLAlchemy Core
    - SQLAlchemy ORM. It's offered as an optioonal library. You don't have to use ORM in order to use the rest of SQLAlchemy.
        - ORM uses the Core library inside.
        - ORM lets you **map** from the _database schema_ to the application's _Python objects_.
        - ORM persists objects into corresponding database tables.

#### SQLAlchemy Layers of Abstraction Diagram

![Image](https://video.udacity-data.com/topher/2019/August/5d4de779_sqlalchemy-layers-of-abstraction/sqlalchemy-layers-of-abstraction.png)

### 8. Mapping Between Tables and Classes

