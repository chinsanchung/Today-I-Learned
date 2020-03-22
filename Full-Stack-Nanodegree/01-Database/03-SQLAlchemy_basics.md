# Nanodgree 2. SQL and Data Modeling for the Web

## lesson 03. SQLAlchemy Basics

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
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://postgres:0000@localhost:5432/example'
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

#### db.create_all()

- db.create_all() detects models for us and creates tables for them (if they don't exist).
- Then, run Flask application.
    - Check Database 'example' with SQL Shell. `\dt`, `\d persons`

#### Auto-incrementing

- We see not null constraints specified on primary key column, and because we specified nulval equal to false, we also have a not null constraint on  our name column.
- SQLAlchemy happens to be smart enough to know that when you're specifying in integer column with primary key equal to true, then it will automatically set the next value of any record as an auto-incremented number.

### 13. Inserting Records, Using Debug Mode

#### Inserting a record

- Run `INSERT INTO persons (name) VALUES ('Amy');` to SQL Shell
- We can fetch that record (Amy) and show it's name.

```python
def index():
    person = Person.query.first()
    return 'Hello ' + person.name
```

#### Debug mode

- These codes will set debug mode to ON, which will automatically restart the server whenever we make changes to our application.
- Command
    - `FLASK_DEBUG=true`
    - `FLASK_DEBUG=true flask run`
    - `$ export FLASK_DEBUG=true` + `$ flask run`

### 14. Experimenting in Interactive Mode

- We can experiment with our app using the [interactive mode](https://docs.python.org/3/tutorial/interpreter.html#interactive-mode) of the Python interpreter.
    - It's a way that you can easily debug and experiment with SQLAlchemy objects and writing SQLAlchemy queries outside of an application using your terminal.

#### Interactive Mode with SQLAlchemy app

- Open terminal, enter 'python3', then it should open up an interactive terminal.
    - In order to import file, the file needs to be an understandable variable name by Python. (not using '-', use '_')
    - you can type exit() in order to exit.
- Input this code to app.py.

```python
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
```

- Then go to Ananconda, type `import <filename>`, `from <filename> import Person`
    - `Person.query.all()` in order to list all of the person objects inside of Person class.
    - You can call other SQLAlchemy methods and objects as well. `Person.query.filter(Person.name == 'Amy')`
    - You can also save results as variable. `query = Person.query.filter(Person.name == 'Amy')`

#### Tips for Debugging SQLAlchemy objects in the Python terminal

- `__repr__`(optional): Ability to customize ap printable string (useful for debugging)
    - You can return a customized string that will print anytime that you're debugging, using Python terminal or anytime you're printing an object in a Python script.
    > `print(person(id=1, name='Amy')) -> <Person 1, 'Amy'>`
- If you want to customize this and go back into Person class an type below.

```python
# db
def __repr__(self):
    return f'<Person {self.id}, {self.name}>'
```

- Then, go terminal, and import again, type print again. It now shows up with our edited string. `<Person ID: 1, name: Amy>`

#### Create redocds in python's interactive mode

- There's another way of inserting redords into database, rather than using `INSERT INTO` SQL commands. It's `db.session.add()`

```bash
$ pythoon3
>>> from app import db, Person
>>> person = Person(name='Max')
>>> db.session.add(person)
>>> db.session.commit()
```

- `db.session.add(person)` will queue up a `INSERT INTO persons (name) VALUES ('Max');` statement in a **transaction** that is managed by `db.session`.
- By `db.session.commit()`, person record will now exist in `persons` table.

### 15. SQLAlchemy Data Types

![data-types](https://video.udacity-data.com/topher/2019/August/5d5a43a0_screen-shot-2019-08-18-at-11.36.57-pm/screen-shot-2019-08-18-at-11.36.57-pm.png)

- There is a one-to-one parity between a SQLAlchemy datatype, and the data type that would be understandable in the semantics of the particular database system that you're linking your SQLAlchemy engine to.
- Calling `db.String()` with no max number of characters passed into it, is the same as declaring a column with data type `VARCHAR`.

### 16. SQLAlchemy Constraints

#### Takeaways

- Column constraints ensure data integrity across database, allowing for database accuracy and consistency.
- Constraints are conditions on your column, that provide checks on the data's validity.
    - It doesn't allow data that violates constraints to be inserted into the database.
- In SQLAlchemy, constraints are set in `db.Column()` after setting the data type.
    - `nullable=False` is equivalent to `NOT NULL` in SQL
    - `unique=True` is equivalent to `UNIQUE` in SQL

```python
# example
class User(db.Model):
    name = db.Column(db.String(), nullable=False, unique=True)
```

#### Implementing a check constraint

```python
class Product(db.Model):
    price = db.Column(db.Float, db.CheckConstraint('price > 0'))
```

### 17. Recap

- SQLAlchemy layers of abstraction

![image1](https://video.udacity-data.com/topher/2019/August/5d5a4854_sqlalchemy-layers/sqlalchemy-layers.png)

![image2](https://video.udacity-data.com/topher/2019/August/5d5a48b0_screen-shot-2019-08-18-at-11.58.46-pm/screen-shot-2019-08-18-at-11.58.46-pm.png)

- Classes vs Tables

![image3](https://video.udacity-data.com/topher/2019/August/5d5a48fb_screen-shot-2019-08-18-at-11.59.53-pm/screen-shot-2019-08-18-at-11.59.53-pm.png)

- db

![image4](https://video.udacity-data.com/topher/2019/August/5d5a4906_screen-shot-2019-08-19-at-12.00.00-am/screen-shot-2019-08-19-at-12.00.00-am.png)