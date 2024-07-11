- Handling User access to the application's data and functionality to prevent users from gaining unauthorized access. 
- Handling user input to the application's functions to prevent malformed input from causing undesirable behavior
- Handling attackers to ensure that the application behaves appropriately when being targeted. Taking suitable defensive and offensive measures to frustrate the attacker.
- Managing the application itself by enabling administrators to monitor its activities and configure its functionality


## Handling User Access
All application needs to control users' access to its data and functionality. A typical situation has many different categories of users. Anonymous ones, ordinary authenticated users, and administrative users. Most web applications handle access using a trio of mechanisms:
- Authentication
- Session Management
- Access Control

Each of these represent a significant area of an application's attack surface. And is fundamental to an application's overall security posture. Because of their interdependencies, the overall security provided by the mechanisms is only as strong as the weakest link in the chain. A defect in any single component may enable an attacker to gain unrestricted access to the application's functionality and/or data. Thus undermining the security posture as a whole. This is why it's important to account for a broad spectrum of possibilities when designing security mechanisms, and to be diligent and test everything as much as possible.

### Authentication
Despite superficial simplicity, authentication mechanisms suffer from a wide range of defects in both design and implementation. Common problems may enable attackers to identify other users' usernames, guess their passwords, or bpyass the login function by exploiting defects in its logic. When you are attacking a web application, you should invest a significant amount of attention to the various authentication related functions it contains. Defects in this functionality enable easy unauthorized access to sensitive data.

### Approaches to Input Handling
Different approaches are commonly taken to the problem of handling user input. A combination of methods is often needed.

**"Reject Known Bad"**
This method employs a blacklist containing a set of literal strings or patterns that are known to be used in attacks. The validation mechanisms blocks any data that matches the blacklist and allows everything else.

- This is generally the least effefctive approach for two reasons:
	- Vulnerability's in web apps can usually be exploited using a variety of input, which may be encoded or represented in a number of different ways.
	- In many cases, it is likely that a blacklist will omit some patterns of input that can be used to attack the application. Second, techniques for exploitation are constantly evolving. 

