Attacker trick's a web browser into performing an unwanted action on a trusted site where the user is authenticated. This is achieved by exploiting the fact that the browser includes any relevant cookies (credentials) automatically, allowing the attacker to forge and submit unauthorized requests on behalf of the user. The attacker's website may contain html forms or JavaScript code that is intended to send queries to the targeted web application.


## Cycle of CSRF's
- Attacker already knows the format of the web application's request to carry out a particular task and sends a malicious link to the user.
- The victim's identity on the Website is verified, typically by cookies transmitted automatically with each domain request and clicks on the link shared by the attacker. This interaction could be a click, mouse over, or any other action.

![phases of csrf](https://tryhackme-images.s3.amazonaws.com/user-uploads/62a7685ca6e7ce005d3f3afe/room-content/61e098019fb1d8611c0aa16f23c34633.svg)

- Insufficient security measures prevent the web application from distinguishing between authentic user requests and those that have been falsified.


### Effects of CSRF
Although CSRF attacks don't directly expose user data, they can still cause harm by changing passwords and email addresses or making financial transactions. The risks associated with CSRF include:
- **Unauthorised Access**: Attackers can access and control a user's actions, putting them at risk of losing money, damaging their reputation, and facing legal consequences.
- **Exploiting Trust**: CSRF exploits the trust websites put in their users, undermining the sense of security in online browsing.
- **Stealthy Exploitation**: CSRF works quietly, using standard browser behaviour without needing advanced malware. Users might be unaware of the attack, making them susceptible to repeated exploitation.


Understanding these risks is essential for implementing effective measures to protect web applications from CSRF vulnerabilities.


## Types of CSRF
Concentrate on state-changing actions carried out by submitting forms. Victim is tricked into submitting a form without realising the associated data like cookies, URL parameters, etc. The victim web browser sends an HTTP request to a web application form where th victim has already been authenticated. These forms are made to transfer money, modify account information, or alter an email address.

![[ef1cd0a1d90c6fbdaeed3b0a29987e01.svg]]

- Victim is already logged onto his bank website. Attackers creafter malicious link and email it to the victim.
- Victim opens the email in the same browser
- Once clicked, the malicious link enables the auto-transfer of the amount from the victim's browser to the attacker's bank account

### XMLHttpRequest CSRF
An asynchronous CSRF exploitation occurs when operations are initiated without a complete page request-response cycle. This is typical of contemporary online apps that leverage asynchronous server communication (via **XMLHttpRequest** or the **Fetch** API) and JavaScript to produce more dynamic user interfaces. These attacks use asynchronous calls instead of the more conventional form submissions. Still, they exploit the same trust relationship between the user and the online service.

An email client for instance, where users change their email preferences without reloading the page. If this online application is CSRF-vulnerable, a hacker might create a fake asynchronous HTTP request, usually a POST request, and alter the victim's email preferences, forwarding all their correspondence to a malicious address.

The following is a simplified overview of the steps that an asynchronous CSRF attack could take: 

- The victim opens a session saved in their browser's cookies and logs into the `mailbox.thm`.  
    
- The attacker entices the victim to open a malicious webpage with a script that can send queries to the `mailbox.thm`.  
    
- To modify the user's email forwarding preferences, the malicious script on the attacker's page makes an AJAX call to `mailbox.thm/api/updateEmail` (using XMLHttpRequest or Fetch).
- The `mailbox.thm` session cookie is included with the AJAX request in the victim's browser.
- After receiving the AJAX request, mailbox.thm evaluates it and modifies the victim's settings if no CSRF defences exist.

### Flash Based CSRF
Flash based CSRF describes the technique of conducting a CSRF attack by taking advantage of flaws in Adobe Flash Player components. Internet applications with features like interactive content, video streaming, and intricate animations hae been made possible with flash. But over time, security flaws in Flash, particularly those that can be used to launch CSRF 