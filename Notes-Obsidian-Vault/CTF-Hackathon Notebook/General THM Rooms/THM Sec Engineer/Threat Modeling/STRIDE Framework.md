## Table of Contents


 - Developed by Microsoft
 - Helps categorize potential security threats

| **Category** | **Definition** | **Policy Violated** |
| ---- | ---- | ---- |
| **Spoofing** | Unauthorised access or impersonation of a user or system. | Authentication |
| **Tampering** | Unauthorised modification or manipulation of data or code. | Integrity |
| **Repudiation** | Ability to deny having acted, typically due to insufficient auditing or logging. | Non-repudiation |
| **Information Disclosure** | Unauthorised access to sensitive information, such as personal or financial data. | Confidentiality |
| **Denial of Service** | Disruption of the system's availability, preventing legitimate users from accessing it. | Availability |
| **Elevation of Privilege** | Unauthorised elevation of access privileges, allowing threat actors to perform unintended actions. | Authorisation |
- As you can see, the table above also provides what component of the CIA triad is violated. The STRIDE framework is built upon this foundational information security concept.

By categorizing threats, organizations can proactively identify and address potential vulnerabilities in their systems, applications, or infrastructure, enhancing their overall security posture.

- **Spoofing** 
    - Sending an email as another user.
    - Creating a phishing website mimicking a legitimate one to harvest user credentials.
- **Tampering**
    - Updating the password of another user.
    - Installing system-wide backdoors using an elevated access.
- **Repudiation**
    - Denying unauthorised money-transfer transactions, wherein the system lacks auditing.
    - Denying sending an offensive message to another person, wherein the person lacks proof of receiving one.
- **Information Disclosure** 
    - Unauthenticated access to a misconfigured database that contains sensitive customer information.
    - Accessing public cloud storage that handles sensitive documents.
- **Denial of Service**  
    - Flooding a web server with many requests, overwhelming its resources, and making it unavailable to legitimate users.
    - Deploying a ransomware that encrypts all system data that prevents other systems from accessing the resources the compromised server needs.
- **Elevation of Privilege**  
    - Creating a regular user but being able to access the administrator console.
    - Gaining local administrator privileges on a machine by abusing unpatched systems.





