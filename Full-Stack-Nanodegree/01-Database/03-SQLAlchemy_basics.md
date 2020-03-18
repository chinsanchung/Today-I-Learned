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

- 