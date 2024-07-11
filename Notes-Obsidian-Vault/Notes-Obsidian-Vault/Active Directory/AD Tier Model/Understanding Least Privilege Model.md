## Least Privilege Model
When implementing a Windows domain infrastructure, correctly assigning privileges to each user/service is difficult. As an admin, some of the question you'll be faced with are:
- Should all/some users have administrative rights over their PCs?
- Should domain administrative users be able to log in to any domain PC?
- Who needs to be part of the Domain Administrators group?
- How should we provide accounts for external users as contractors?

the **least privilege model** consists of assigning each user/service strictly the privileges they need to perform their usual tasks and nothing more.

Having our identity management built upon the model is advantageous for the follow reasons:
- We will stop legitimate users from abusing privileges to access sensitive information.
- If a user is compromised, attackers will be heavily limited to only performing tasks permitted to the compromised user. This will prevent or at least complicate lateral movment and privilege escalation techniques.
- Is a user executes malware, its propagation will likely be limited by the strict privileges of the compromised account.

## The Problem With Excessive Privileges

As networks grow, it is common to find accounts with more privileges than they should. There are many reasons why this may happen, including:

- Users may be assigned privileges individually rather than based on their roles. This becomes hard to maintain as the company grows.
- Privileges aren't revoked for users who leave the company.
- Users who have moved through different positions in the company may still carry the privileges of their older roles. This problem is known as **privilege creep**.
- No efforts have been made to reduce the default privileges assigned to users in the network.

A significant number of overly privileged accounts can quickly become a problem. To get a better idea of why, take the following scenario into account:

1. An attacker gains an initial foothold into your network using a recently published zero-day exploit. As a result, he gets local administrative privileges on the compromised host.
2. Using tools like `mimikatz`, the attacker dumps the cached credentials of users who have recently logged into the machine. One of such credentials corresponds to the user "**John**".
3. The attacker realises that "**John**" is a member of the Helpdesk group. Because of poor security practices, Helpdesk users have administrative privileges over all machines in the domain.
4. The attacker now has administrative access to all computers through John's account.
