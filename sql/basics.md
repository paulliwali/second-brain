- Watching [video](https://www.youtube.com/watch?v=HXV3zeQKqGY) from freeCodeCamp

# Introduction to database

- A database management system (DBMS) is used to manage databases
  - Including MS SQL, MYSQL, PostgreSQL (object oriented), Oracle, SQLite
  - Perform CRUD
- Two main types of databases are **relational (SQL) and non-relational (noSQL)**
  - Relational DB organizes data in tables
  - Non-relational DB organizes data into key-value pairs, json, XML, graphs, or flexible tables
- Relational DBMS include mySQL, Oracle, postgreSQL
  - Uses **structured query language (SQL)** to interact 
  - SQL code is not always portable to another RDBMS
- Non-relational DBMS include mongoDB, apache cassandra, firebase
  - No set language to interact with all NRDBMS because they are implemented differently

# Tables and Keys

- Tables should have
  - Column(s) which defines a property
  - Row(s) which defines an entry
  - **Primary key** which uniquely defines the rows of the database, see **index**
    - A **surrogate key** is not mapped to anything
    - A **natural key** is mapped to some real world meaning
    - A **foreign key** is mapped to another table's primary key in the DB
    - A **composite key** is comprised of multiple columns when used in conjunction uniquely identifies the row

# SQL Basics

- SQL implementation is different across different RDBMS but shares the core concepts
- A hybrid language that combines data query language (DQL), data definition language (DDL), data control language (DCL), data manipulation language (DML)
  - DQL is used to query for information
    - To get the *exact* information that you want and *nothing more*
  - DDL is used to define schemas
  - DCL is used to control access and manage security
  - DML is used to for C(R)UD operations
- Syntax convention to write SQL keywords in all caps

## Create Tables

- First thing is to define a schema
  - `INT`
  - `DECIMAL(M,N)`: M total digits, N digits after decimal
  - `VARCHAR(l)`: String of text with l length
  - `BLOB:` binary large objects
  - `DATE`: 'YYYY-MM-DD'
  - `TIMESTAMP`: 'YYYY-MM-DD HH:MM:SS'
- Columns can have **constraints** to help with incomplete information
  - If column should be `NULL` or `NOT NULL`
  - If column should be `UNIQUE`
  - The `DEFAULT` value if nothing is entered
  - Primary key can be `AUTO_INCREMENT` which is useful for integer primary keys
- `FOREIGN KEY` to add the foreign key
  - To add values in foreign key columns, they have to already exist in order to be referenced
- `ON DELETE SET NULL` and `ON DELETE SET CASCADE` to take care of what happens to foreign keys

## Updating and Deleting Tables

- `UPDATE`... `SET`... `WHERE`...
  - Update column(s) of specific row(s) that pass the condition(s)
- `DELETE FROM`... `WHERE`...
  - Delete row(s) that pass the condition(s)

## Queries

- `SELECT`... `FROM`... 
  - to retrieve the columns
- `LIMIT` is used to filter the number of rows that is returned
- `WHERE` is used to construct filters
  - Can get complex using boolean comparisons: `<, >, <=, >=, =, <>, AND, OR`
  - `IN` is used to make 1-to-many comparisons
- Wildcards include `%` for any number of characters and `_` for one character

## Operations

- `UNION` combines rows from tables
  - They have to have the same data type and dimensions
- `JOIN` combines columns from tables
  - They have to share key columns

## ER Diagrams
- **Entity** is an object to model about
  - **Weak entity** is an entity that can't be uniquely identified by its attributes, will depend on another entity - *thus full participation on the entity*
- **Attributes** info about the entry
  - **Composite attributes** can be broken into sub-attributes
  - **Multi-valued attributes** can have more than one value (double outline)
  - **Derived attributes** are "calculated" from other attributes (dashed outline)
- **Relationships** defines a connection between two entities
  - **Relationship attribute** are defined only on the relationship
  - **Relationship cardinality** are 1-to-1, 1-to-many, many-to-many
- Converting ER diagrams to schema:
  1. Map regular entity types to tables with entities as columns
  2. Map weak entity types with composite key of primary key of owner and itself
  3. Map binary 1-to-1 relationships by including one side of the relationship's primary key as foreign keys in favor of total participation
  4. Map binary 1-to-many relationships by including one side of the relationship's primary key as foreign keys on the "many" side
  5. Map binary many-to-many relationships by creating a new table with composite key with primary key of both sides and any attributes
