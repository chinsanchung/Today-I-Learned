# Nanodgree 2. SQL and Data Modeling for the Web

## chapter 02. Interacting with Databases

### 1. Recap: Relational Databases

#### definitions

- A database is a collection of data
- A database system is a system for storing collections of data in some organized way

#### databases

- Persistence (allowing access later, after it was created)
- Shared source of truth : accessible by many users
- Ability to store many types of data (efficiently)
- Concurrency control (handling multiple DB actions at once)
- Split between relational and nonrelational database management systems

#### Relational Databases
- All data is stored in tables
- Every table is characterized by a list of columns with data types per column, and its set of data (organized in rows)
- Comes with rules for enforcing data integrity, such as constraints and triggers

### 2. Primary Keys & Foreign Keys

#### Primary Key

- Every row in a table should have a column or set of columns that is a unique identifier for the entire row.
- The primary key is defined as one or more columns that uniquely identifies the whole row.
- If there are more multiple columns for the primary key, then the set of primary key columns is known as a composite key.

#### Foreign Key

- A primary key in another (foreign) table.
- Foreign keys are used to map relationships between tables.
- A foreign key maps two primary keys, encodling a relationship from one table to another. It must be a primary key of another table.

### 3. SQL
- Every relational database stystem has its own "flavor" of SQL it implements.
- Each flavor is called a **dialect** that is unique per database management system.

#### Command

- Manipulating Data
    - INSERT
    - UPDATE
    - DELETE
- Querying Data
    - SELECT
- Structuring Data
    - CREATE TABLE
    - ALTER TABLE
    - DROP TABLE
    - ADD COLUMN
    - DROP COLUMN
- joins & Groupings
    - INNER JOIN, OUTER JOINS (LEFT, RIGHT)
    - GROUP BY, SUM, COUNT

### 4. Execution Plan

- The DBMS takes a SQL query and generates an **execution plan** for the database engine to follow. `"SELECT * from vehicles WHERE driver_id = 3"`
- Since there can multiple ways of performing queries against a database with various performance tradeoffs, it's important to choose the one with the best execution plan for quickly and efficiently returning the results that you need.
- For a `SELECT *` operation, the most basic operation is Seq Scan.
    - It opens the file storing the table, then reads each rows, one by one, returning them to user.
- We should generally know that this is what we do a SELECT statement.
    > SELECT make model FROM vehicles JOIN drivers ON vehicles.drver_id = drivers.id
    - Hash join: joins two recode sets. The hash join creates a hash in-memory that hashes based the driver_id column.
    - Seq Scan: a sequential scan is done across the entire vehicles table. This makes sense since we're looking to fetch all make and model information across all records in the vehicles table.
    - Hash with Seq Scan on drivers: Join key is checked in the Hash returned from Step 1, whereif it doesn't exist, given that is an Inner join. We fetch the row from the hash to generate the outputted, joind row.
- [Use the Index, Luke!](https://use-the-index-luke.com/) is a highly in-depth guide to database performance for developers.

### 5. Client-Server Model

In order to build database-backed web applications, we first need to understand how servers, clients, and databases interact.

- A major part of this is a **client-server model**. Modern-day database systems follow the client-server model for interacting with server.
    - A **server** is a centralized program that communicates over a network (such as the Internet) to serve _clients_.
    - And a **client** is a program (like the web browser on your computer) that can request data from a server.
    - When you go to a web page in your browser, your browser (the client) makes a request to the server-- which then returns the data from that page.

#### Adding databases to the model

- Servers, Clients, Hosts
    - In a Client-Server Model, a **server** serves many **clients**.
    - Servers and clients are programs that run on hosts.
    - **Hosts** are computers connected over a network (like the internet!).
- Requests and Responses
    - A client sends a request to the server
    - The server's job is to fulfill the request with a response it sends back to the client.
    - Requests and responses are served via a **communication protocol**, which sets up the expectations and rules for how the communication occurs between servers and clients.
- Relational Database Clients
    - A database client is any program that sends requests to a database
    - In some cases, the database client is a web server! When your browser makes a request, the web server acts as a server (fulfilling that request), but when the web server requests data from the database, it is acting as a client to that database—and the database is the server (because it is fulfilling the request).
    - Don't let this confuse you. To a database server (It serves response), a web server is a Client (It makes a request).

### 6. Client-Server Model Example: Jane's Store

#### Example: Shopping site's process

```javascript
// REQUEST
// <div id="polo" data-id="1">Polo</div>
document.getElementById('polo').onclick = function() {
    var request = new Request();
    request.send('GET product detail on product with id', 1);
}
```
```python
# SERVER
result = application.listen_on('GET product detail on product with id')

print(result.id) #1 이 나와야함
## get more infomation

application.render_view('product_detail.html', data)
```
- Takeaways
    - Clicking on the Polo product leads to a _click event_ being registered by the browser, on the client computer.
    - A click handler in the view would _send a request to the server_ (in Javascript) from the client browser.
        - A client could request more data and a different view to be rendered (with that data).
    - A server process _listens to the request_ sent from the view. It fetches the data and chooses what to render next, using the fetched data.
- Takeaways
    - _The client sends a request to the server_, including information about the request type and any user input data.
    - _The server receives the request_, and uses the user input data to determine how to shape its request to the database, and _sends a request to the database_.
    - _The database processes this request_, and _sends a response back_ to the web server.
    - The server _receives the response_ from the database, and uses it to determine the view + powers the view template with the fetched data, sending it back to the client's browser.
    - The client is _responsible for rendering something to the user_, that represents both the data and its representation.
- summary
    > The **web server** receives a request from the **client** and sends a request to the **database**, which sends back a response to the **web server**, **which then sends back a response to the client**.

### 7. TCP/IP

- Postgres follows a client-server model that supports TCP/IP for communication.

#### Transmission Control Protocol and Internet Protocol

- TCP/IP is a suite of communication protocols that is used to connect devices and transfer data over the Internet.
- TCP/IP uses two things.
    - IP Address: Identifying the location of a computer on a network.
        - IP Address has many ports. (total 65,535 TCP ports on a given computer)
    - Ports: A port is a location on the recipient computer, where data is received.
        - Ports allow multiple types of traffic being received at the smae time on a given device, to be tracked and routed to where they need to go.
        - Ports are much like the different terminals of an airport, tracking and receiving different airplanes at the same time, allowing for the effective receipt of multiple types of traffic at the **same** IP address.
- While an IP address tells you where to find a particular computer, it doesn't tell you specifically where on that computer a particular connection should be made. --that's what **port numbers** are for.

#### Some Port numbers you should know

- Port 80: It most commonly used for _HTTP requests_.
- Port 5432: It used by most database systems that listen on to receive traffic and also send back information to their clients; default port for Postgres.

### 8. Connections and Sessions in TCP/IP

#### Takeaways

- TCP/IP is **connection-based** protocol, meaning all communications between parties are arranged over a connection.
     - To start any communication, a connection must be arranged.
     - Open a connection to start communications, Close a connection to end communications.
- Connecting starts a session, Ending the connection ends the session.
    - **session**: It is essentially a period between the start and end of a connection that we may have between a client and a server.
    - Within a session, we encapsulate interactions between a client and a sever in units called **transactions**.
- In a database session, many **transactions** can occur during a given session. Each transaction does work to commit changes to the database(updating, inserting, or deleting records).

#### UDP

- UDP is much simpler than TCP. Hosts on the network send data without any connections needing to be established.
- TCP vs UDP
    - When speed is more important than reliability, especially when applications need to stream very small amounts of information quickly, then UDP is preferred. (e.g. live TV streaming, VoIP)
    - Since UDP does not need to retransmit lost datagrams, nor does it do any connection setup, there are fewer delays over UDP than TCP.
    - TCP's continuous connection is more reliable but has more latency.

### 9. Transactions

Transactions are atomic **units of work** for the database to perform as a whole.

#### Relational databases are transactional

- All changes to data are made through units called transactions.
    - Either a single change or multiple changes. (UPDATE, INSERT, DELETE)
    - Executed in an ordered sequence.
- An operation that either succeeds altogether, or fails altogether as a unit.
    - one or more UPDATE, INSERT, DELETE statements can be added to it. Then you can send off the set of changes to the database by _committing_ the transaction.
    - Transaction can be cleared of commands using a _rollback_.
- Why bunlde work into transactions?
    - Database systems can fail
    - We want the database to always be in a valid state

#### Takeaways

- Databases are interacted using client-server interactions, over a network.
- Postgres uses TCP/IP to be interacted with, which is connection-based.
- We interact with databases like Postgres during sessions.
- Sessions have transactions that commit work to the database.

### 10. Installing Postgres

[download](https://www.postgresql.org/download/)

### 11. Postgres Command Line Applications

#### Postgres CLI tools

- Login as a particular user `sudo -u <username> -i`
    - Default installed user is called "postgres"
- Create a new database `createdb <database_name>`
- Destroy a database `dropdb <database_name>`
- Reset a database `dropdb <database_name> && createdb <database_name>`

### 12. Intro to psql

#### Takeaways

- **psql** is an interactive terminal application for connecting and interacting with your local postgres server on your machine.
- Connect using `$ psql <dbname>`
- psql lets you
    - Directly type and execute SQL commands
    - Inspect and preview your database and database tables using psql meta-commands
- Run these codes in SQL Shell
#### Useful basic psql commands

- `psql <dbname> [<username>]`: Starts psql with a connection to dbname. Optionally use another user than current user.
    - If you need to login as a diferent user, try this command. (ex: `psql testdb testName`)
- `# \l`: List all databases on the server, their onwers, and user access levels.
- `# \c <dbname>`: Connect to a database named.
- `# \dt`: Show database tables.
- `# \d <tablename>`: Describe table schema.
- `# \q`: Quit psql, return to the terminal.
- `# \?`: Get help

### 13. Using psql - SQL Commands

### 14. Using psql - Final remarks

[postgres commands](https://video.udacity-data.com/topher/2019/August/5d5a1055_postgres-psql-cheat-sheet/postgres-psql-cheat-sheet.pdf)

### 15. Other Postgres Clients

#### postgres clients

- Command Line Tools: psql, createdb, dropdb
- GUIs: pgAdmin, PopSQL (for macOS)
- Database Adapters: psycopg2 (to make client Python apps)

#### Default Postgres Connection Settings

- Host: localhost (aka, 127.0.0.1)
- Port: 5432
- Username: postgres
- Password: (None)

### 16. DBAPIs and psycopg2

#### Takeaways

- What is a DBAPI?
    - A DBAPI provides a standard interface for one programming language (like python) to talk to a relational database server.
    - Low level library for writing SQL statements that connect to a database.
    - Also known as database adapters.
- Different DBAPIs exist for every server frameworkor language + database system.
- Database adapters define a standard for using a database and using the results of database queries as input data in the given language. (ex: js->array, python->tuples)

### 17. psycopg2: basic usage

```python
import psycopg2

conn = psycopg2.connect('dbname=todoapp_development user=amy')

cursor = conn.cursor()

# Open a cursor to perform database operations
cur = conn.cursor()

# drop any existing todos table
cur.execute("DROP TABLE IF EXISTS todos;")

# (re)create the todos table
# (note: triple quotes allow multiline text in python)
cur.execute("""
  CREATE TABLE todos (
    id serial PRIMARY KEY,
    description VARCHAR NOT NULL
  );
""")

# commit, so it does the executions on the db and persists in the db
conn.commit()

cur.close()
conn.close()
```

### 18. psycopg2: string composition

#### Takeaways

- We can use **string interpolation** to compose a SQL query using python strings.
    - Using `%s`, passing in a tuple as the 2nd arguments in `cursor.execute()`
    - Using named string parameters `%(foo)s`, passing in a dictionary instead.

```python
import psycopg2

connection = psycopg2.connect('dbname=example')

cursor = connection.cursor()

cursor.execute('DROP TABLE IF EXISTS table2;')

cursor.execute('''
  CREATE TABLE table2 (
    id INTEGER PRIMARY KEY,
    completed BOOLEAN NOT NULL DEFAULT False
  );
''')

cursor.execute('INSERT INTO table2 (id, completed) VALUES (%s, %s);', (1, True))

SQL = 'INSERT INTO table2 (id, completed) VALUES (%(id)s, %(completed)s);'

data = {
  'id': 2,
  'completed': False
}
cursor.execute(SQL, data)

connection.commit()

connection.close()
cursor.close()
```

### 19. psycopg2: fetching results

```python
cursor.execute('SELECT * from table2;')
result = cursor.fetchall()
print(result)
```
#### Steps for getting a database-backed web application up and running

1. Create a database: `createdb`
2. Establish a connection to the database: `psycopg2.connect()`
3. Define and create your data schema: `CREATE TABLE`
4. Seed the database with inital data: Optional.
5. Create routes and views: Write up HTML, CSS, JavaScript in our views.
6. Run the server
7. Deploy the server to the web
