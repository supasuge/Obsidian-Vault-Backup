## Table of Contents



`9. What attack forces an end user to execute unwanted actions on a web application in which they're currently authenticatd?`
CSRF


**Characterize the major flaw in the following code excerpt:**
```sql
string query = "SELECT account_balance FROM user_data WHERE user_name = "
								+ request.getParameter("customerName");
Try {
	Statement statement = connection.createStatement( . . . );
	ResultSet results = statement.executeQuery( query );
}
```
A: Unvalidated "CustomerName" parameter allowing remote file inclusion


In the code excerpt below, which defense technique is applied to defend against SQL injection attacks?  
  
```sql
String custname = request.getParameter("customerName");  
try {  
    CallableStatement cs = connection.prepareCall("{call sp_getAccountBalance(?)}");  
    cs.setString(1, custname);  
    ResultSet results = cs.executeQuery();  
}  catch (SQLException se)
```

`Use of Stored Procedures`


The following string is an example of which type of attack?  
  
```HTML
<img src="http://www.example.com/forumpost.php?msg=Pwned">
```
`Cross-Site Scripting (XSS)`


The following URL shows an attacker trying to exploit what vulnerabilty?
`http://example.com/?file=../../../../etc/passwd
A: `Local File Inclusion`


What is the maximum number of characters that can be stored in the quest table's "path" column?
`30`

```sql
varchar(30);
```













