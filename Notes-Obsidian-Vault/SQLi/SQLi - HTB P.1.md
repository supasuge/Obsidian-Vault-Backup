## Table of Contents

  - [Architecture](#Architecture)
- [Types of Databases](#types\of\databases)
  - [Non-relational Databases](#Non-relational\Databases)
- [Intro to MySQL](#intro\to\mysql)
  - [Structured Query Language (SQL)](#Structured\Query\Language\(SQL))
  - [Command Line](#Command\Line)
  - [Creating a database](#Creating\a\database)
  - [Tables](#Tables)
      - [Table Properties](#Table\Properties)

![[db_request_3.webp]]

SQL injection refers to attacks against relational databases such as `MySQL` (whereas injections against non-relational databases, such as MongoDB, are NoSQL injection). This module will focus on `MySQL` to introduce SQL Injection concepts.

There are many ways to accomplish this. To get a SQL injection to work, the attacker must first inject SQL code and then subvert the web application logic by changing the original query or executing a completely new one. First, the attacker has to inject code outside the expected user input limits, so it does not get executed as simple user input. In the most basic case, this is done by injecting a single quote (`'`) or a double quote (`"`) to escape the limits of user input and inject data directly into the SQL query.

Once an attacker can inject, they have to look for a way to execute a different SQL query. This can be done using SQL code to make up a working query that executes both the intended and the new SQL queries. There are many ways to achieve this, like using [stacked](https://www.sqlinjection.net/stacked-queries/) queries or using [Union](https://www.mysqltutorial.org/sql-union-mysql.aspx/) queries. Finally, to retrieve our new query's output, we have to interpret it or capture it on the web application's front end.

|**Feature**|**Description**|
|---|---|
|`Concurrency`|A real-world application might have multiple users interacting with it simultaneously. A DBMS makes sure that these concurrent interactions succeed without corrupting or losing any data.|
|`Consistency`|With so many concurrent interactions, the DBMS needs to ensure that the data remains consistent and valid throughout the database.|
|`Security`|DBMS provides fine-grained security controls through user authentication and permissions. This will prevent unauthorized viewing or editing of sensitive data.|
|`Reliability`|It is easy to backup databases and rolls them back to a previous state in case of data loss or a breach.|
|`Structured Query Language`|SQL simplifies user interaction with the database with an intuitive syntax supporting various operations.|

## Architecture

The diagram below details a two-tiered architecture.

![dbms_architecture](https://academy.hackthebox.com/storage/modules/33/db_2.png)

`Tier I` usually consists of client-side applications such as websites or GUI programs. These applications consist of high-level interactions such as user login or commenting. The data from these interactions is passed to `Tier II` through API calls or other requests.

The second tier is the middleware, which interprets these events and puts them in a form required by the DBMS. Finally, the application layer uses specific libraries and drivers based on the type of DBMS to interact with them. The DBMS receives queries from the second tier and performs the requested operations. These operations could include insertion, retrieval, deletion, or updating of data. After processing, the DBMS returns any requested data or error codes in the event of invalid queries.


It is possible to host the application server as well as the DBMS on the same host. However, databases with large amounts of data supporting many users are typically hosted separately to improve performance and scalability.

# Types of Databases

---

Databases, in general, are categorized into `Relational Databases` and `Non-Relational Databases`. Only Relational Databases utilize SQL, while Non-Relational databases utilize a variety of methods for communications.

## Non-relational Databases

A non-relational database (also called a `NoSQL` database) does not use tables, rows, and columns or prime keys, relationships, or schemas. Instead, a NoSQL database stores data using various storage models, depending on the type of data stored. Due to the lack of a defined structure for the database, NoSQL databases are very scalable and flexible. Therefore, when dealing with datasets that are not very well defined and structured, a NoSQL database would be the best choice for storing such data. There are four common storage models for NoSQL databases:

- Key-Value
- Document-Based
- Wide-Column
- Graph

Each of the above models has a different way of storing data. For example, the `Key-Value` model usually stores data in JSON or XML, and have a key for each pair, and stores all of its data as its value: ![HTML Example](https://academy.hackthebox.com/storage/modules/75/web_apps_non-relational_db.jpg)


The above example can be represented using JSON as:

Code: json

```json
{
  "100001": {
    "date": "01-01-2021",
    "content": "Welcome to this web application."
  },
  "100002": {
    "date": "02-01-2021",
    "content": "This is the first post on this web app."
  },
  "100003": {
    "date": "02-01-2021",
    "content": "Reminder: Tomorrow is the ..."
  }
}
```

# Intro to MySQL

---

This module introduces SQL injection through `MySQL`, and it is crucial to learn more about `MySQL` and SQL to understand how SQL injections work and utilize them properly. Therefore, this section will cover some of MySQL/SQL's basics and syntax and examples used within MySQL/MariaDB databases.

---

## Structured Query Language (SQL)

SQL syntax can differ from one RDBMS to another. However, they are all required to follow the [ISO standard](https://en.wikipedia.org/wiki/ISO/IEC_9075) for Structured Query Language. We will be following the MySQL/MariaDB syntax for the examples shown. SQL can be used to perform the following actions:

- Retrieve data
- Update data
- Delete data
- Create new tables and databases
- Add / remove users
- Assign permissions to these users

## Command Line

The `mysql` utility is used to authenticate to and interact with a MySQL/MariaDB database. The `-u` flag is used to supply the username and the `-p` flag for the password. The `-p` flag should be passed empty, so we are prompted to enter the password and do not pass it directly on the command line since it could be stored in cleartext in the bash_history file.

```shell
gdxqpardo@htb[/htb]$ mysql -u root -p
```

Again, it is also possible to use the password directly in the command, though this should be avoided, as it could lead to the password being kept in logs and terminal history:

```shell
gdxqpardo@htb[/htb]$ mysql -u root -p<password>
```

The examples above log us in as the superuser, i.e.,"`root`" with the password "`password`," to have privileges to execute all commands. Other DBMS users would have certain privileges to which statements they can execute. We can view which privileges we have using the [SHOW GRANTS](https://dev.mysql.com/doc/refman/8.0/en/show-grants.html) command be discussed later.

When we do not specify a host, it will default to the `localhost` server. We can specify a remote host and port using the `-h` and `-P` flags.

```shell-session
gdxqpardo@htb[/htb]$ mysql -u root -h docker.hackthebox.eu -P 3306 -p 

Enter password: 
...SNIP...

mysql> 
```
## Creating a database

Once we log in to the database using the `mysql` utility, we can start using SQL queries to interact with the DBMS. For example, a new database can be created within the MySQL DBMS using the [CREATE DATABASE](https://dev.mysql.com/doc/refman/5.7/en/create-database.html) statement.

Intro to MySQL

```shell
mysql> CREATE DATABASE users;
```

SQL statements aren't case sensitive, which means 'USE users;' and 'use users;' refer to the same command. However, the database name is case sensitive, so we cannot do 'USE USERS;' instead of 'USE users;'. So, it is a good practice to specify statements in uppercase to avoid confusion

## Tables

DBMS stores data in the form of tables. A table is made up of horizontal rows and vertical columns. The intersection of a row and a column is called a cell. Every table is created with a fixed set of columns, where each column is of a particular data type.

A data type defines what kind of value is to be held by a column. Common examples are `numbers`, `strings`, `date`, `time`, and `binary data`. There could be data types specific to DBMS as well. A complete list of data types in MySQL can be found [here](https://dev.mysql.com/doc/refman/8.0/en/data-types.html). For example, let us create a table named `logins` to store user data, using the [CREATE TABLE](https://dev.mysql.com/doc/refman/8.0/en/creating-tables.html) SQL query:

```sql
CREATE TABLE logins (
    id INT,
    username VARCHAR(100),
    password VARCHAR(100),
    date_of_joining DATETIME
    );
```

As we can see, the `CREATE TABLE` query first specifies the table name, and then (within parentheses) we specify each column by its name and its data type, all being comma separated. After the name and type, we can specify specific properties, as will be discussed later.

Intro to MySQL

```shell
mysql> CREATE TABLE logins (
    ->     id INT,
    ->     username VARCHAR(100),
    ->     password VARCHAR(100),
    ->     date_of_joining DATETIME
    ->     );
Query OK, 0 rows affected (0.03 sec)
```

The SQL queries above create a table named `logins` with four columns. The first column, `id` is an integer. The following two columns, `username` and `password` are set to strings of 100 characters each. Any input longer than this will result in an error. The `date_of_joining` column of type `DATETIME` stores the date when an entry was added.

A list of tables in the current database can be obtained using the `SHOW TABLES` statement. In addition, the [DESCRIBE](https://dev.mysql.com/doc/refman/8.0/en/describe.html) keyword is used to list the table structure with its fields and data types.

```shell-session
mysql> DESCRIBE logins;

+-----------------+--------------+
| Field           | Type         |
+-----------------+--------------+
| id              | int          |
| username        | varchar(100) |
| password        | varchar(100) |
| date_of_joining | date         |
+-----------------+--------------+
4 rows in set (0.00 sec)
```

#### Table Properties

Within the `CREATE TABLE` query, there are many [properties](https://dev.mysql.com/doc/refman/8.0/en/create-table.html) that can be set for the table and each column. For example, we can set the `id` column to auto-increment using the `AUTO_INCREMENT` keyword, which automatically increments the id by one every time a new item is added to the table:

```sql
  id INT NOT NULL AUTO_INCREMENT,
```


The `NOT NULL` constraint ensures that a particular column is never left empty 'i.e., required field.' We can also use the `UNIQUE` constraint to ensures that the inserted item are always unique. For example, if we use it with the `username` column, we can ensure that no two users will have the same username:

```sql
  username VARCHAR(100) UNIQUE NOT NULL,
```

Another important keyword is the `DEFAULT` keyword, which is used to specify the default value. For example, within the `date_of_joining` column, we can set the default value to [Now()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_now), which in MySQL returns the current date and time:

Code: sql

```sql
date_of_joining DATETIME DEFAULT NOW(),
```

Finally, one of the most important properties is `PRIMARY KEY`, which we can use to uniquely identify each record in the table, referring to all data of a record within a table for relational databases, as previously discussed in the previous section. We can make the `id` column the `PRIMARY KEY` for this table:

```sql
PRIMARY KEY(id)
```

The final `CREATE TABLE` query will be as follows:

Code: sql

```sql
CREATE TABLE logins (
    id INT NOT NULL AUTO_INCREMENT,
    username VARCHAR(100) UNIQUE NOT NULL,
    password VARCHAR(100) NOT NULL,
    date_of_joining DATETIME DEFAULT NOW(),
    PRIMARY KEY (id)
    );
```


