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

#### Class Example

```python
# Human class allows us to instantiate an instance of a human being.
class Human:
    def __init__(self, first_name, last_name, age):
        self.first_name = first_name
        self.last_name = last_name
        self.age = age
#  Create object instances of the Human class
sarah = Human('Sarah', 'Silverman', 48)
bob = Human('Bob', 'Seget', 54)
```
```SQL
-- Instantiate a table schema in a Relational Database System
CREATE TABLE humans (id INTEGER PRIMARY KEY, first_name VARCHAR, last_name VARCHAR, age INTEGER);
```

#### Takeaways

- Tables map to classes.
- Table records map to class objects.
- Table columns map to class attributes within that class.

### 9. Hello App with Flask-SQLAlchemy - Part 1

#### Flask

- A very simple web framework for serving web pages with data.
    > pip3 install flask
    > from flask import Flask

#### Flask-SQLAlchemy

- Flask-SQLAlchemy is a Flask extension that supports SQLAlchemy.
    > pip3 install flask-sqlalchemy
    > from flask_sqlalchemy import SQLAlchemy

#### Initializing the app

- Goal: Create a Hello app that says 'Hello' to your name, stored in a table on a database
- Before initializing app [from: opentutorials.org](https://www.opentutorials.org/module/3669/22002)
    - Install virtualenv `pip install virtualenv`
    - Move to directory, then command `virtualenv venv`
    - If you want to close virtualenv, command `deactivate`
- Initialize

```python
# app.py
from flask import Flask

# instance of Flask. This is a standard way for creating a flask application.
app = Flask(__name__)
# Python decorator. Tell Flask app which endpoint to listen to for connections.
@app.route('/')
def index():
    return 'Hello World!'

if __name__ == '__main__':
    app.run()
```

- Run App
    - `FLASK_APP=app.py FLASK_DEBUG=true flask run`
    - with `if __name__ == '__main__'`, `python app.py`

### 10. Connecting to the Database

#### Database Connection URI Parts

![image](https://classroom.udacity.com/nanodegrees/nd0044/parts/216c669c-5e62-43a1-bcb9-8a8e5eca972a/modules/43f34772-8032-4851-938b-d952bbfc7f1c/lessons/53139bef-389d-4d0a-b7b6-e4bb4bd0ef0c/concepts/86d59d1d-6b21-479f-b1a6-2ecfe983736d#)

- We need to pass in a number of parameters for SQLAlchemy to understand how to connect to our database and which database to use.
    - host: name of the host location where your database is located.

#### Connect with Flask-SQLAlchemy

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
# instance of a database that we can interact with in SQLAlchemy land to Flask app.
db = SQLAlchemy(app)
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://postgres@localhost:5432/example'
@app.route('/')
@app.route('/index')
def index():
    return 'Hello Flask!'

```

- All configuration variables in a Flask App are set on the dictionary app.config
    - Flask-SQLAlchemy expects a configuration variable called 'SQLALCHEMY_DATABASE_URI'

### 11. db.Model and Defining Models

#### Create records within a table

```python
db = SQLAlchemy(app)
```
- What db offers us is an interface by which we can use SQLAlchemy underneath. (interacting with database)
- In particular, db offers us several objects.
    - db.Model: Ability to create and manipulate data models.
    - db.session: Ability to create and manipulate database transactions.

#### Create a person class with db.Model
```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://postgres@localhost:5432/example'
db = SQLAlchemy(app)

class Person(db.Model):
  __tablename__ = 'persons'
  id = db.Column(db.Integer, primary_key=True)
  name = db.Column(db.String(), nullable=False)
```
#### Takeways

- Declaring classes
    - class MyModel(db.Model) will inherit from db.Model
    - By inheriting from db.Model, we map from our classes to tables via SQLAlchemy ORM
- Defining columns
    - Within our class, we declare attributes equal to db.Column
    - db.Column takes `<datatype>, <primary_key?>, <constraint?>, <default?>`
- Table naming
    - By default, SQLAlchemy will pick the name of the table, setting it equal to the lower-case version of your class's name. Otherwise, we set the name using `__tablename__='custom_name'`

### 12. Syncing Models, db.create_all()
