## Table of Contents

          - [Key Terms/Ideas](#Key\Terms/Ideas)
    - [Main Goal](#Main\Goal)
    - [Collaboration with Different Teams](#Collaboration\with\Different\Teams)
    - [Attack Trees](#Attack\Trees)
- [Modeling With MITRE ATT&CK](#modeling\with\mitre\att&ck)
- [DREAD Framework](#dread\framework)
- [STRIDE Framework](#stride\framework)
  - [Threat Modeling with STRIDE](#Threat\Modeling\with\STRIDE)
          - [Skill Assessment](#Skill\Assessment)
- [PASTA Framework](#pasta\framework)
    - [Seven-Step Methodology](#Seven-Step\Methodology)
    - [Application of PASTA Framework](#Application\of\PASTA\Framework)
      - [Business Analyst](#Business\Analyst)
      - [Network Engineer](#Network\Engineer)

###### Key Terms/Ideas
Identifying, prioritizing, and addressing potential security threats across the organization. 

### Main Goal 
Reduce the organisation's risk exposure. 
**Threat, Vulnerability, and Risk**
*Threat*: Refers to any potential occurrence, event, or actor that may exploit vulnerabilities to compromise information confidentiality, integrity, or availility
*Vulnerability*: A weakness of flaw ina  system, application, or process that may be exploited by a threat to cause harm. may come from software bugs, misconfigurations, or design flaws.

*Risk*: The possibility of being compromised because of a threat taking advantage of a vulnerability.  A way to think about hwo likely an attack might be successful and how much damage it could cause

1. Define the scope
	- Identify the specific systems, applications, and networks in the threat modeling exercise
2. Asset Identification
	- Develop diagrams of the organisation's architecture and its dependencies. It is also essential to identify the importance of each asset nased on the information it handles, such as customer data, intellectual property
3. Identify threats
	- Identify potential threats that may impact the identified assets, such as cyber attacks, physical attacks, social engineering, and insider threats
4. Analyse Vulnerabilities and Prioritise Risks
	- Analyse the vulnerabilities based on the potential impact of identified threats in conjunction with assessing the existing security controls. Given the list of said vulnerabilites; risks should be prioritised based on their likelihood of successful exploitation and potential impact.
5. Develop and implement Countermeasures
	- Design and implement security controls to address the identified risks, such as implementing access controls, applying system updates, and performing regular vulnerability assessments.
6. Monitor and Evaluate
	- Continuously test and monitor the effectiveness of the implemented countermeasures and evaluate the success of the threat modelling exercise. An example of a simple measurement of success is tracking the identified risks that have been effectively mitigated or eliminated.


### Collaboration with Different Teams
| Team | Role and Purpose |
|:-:|:-:|
|**Security Team**|The overarching team of red and blue teams. This team typically lead the threat modelling process, providing expertise on threats, vulnerabilities, and risk mitigation strategies. They also ensure security measures are implemented, validated, and continuously monitored.|
|**Development Team**|The development team is responsible for building secure systems and applications. Their involvement ensures that security is always incorporated throughout the development lifecycle.|
|**IT and Operations Team**|IT and Operations teams manage the organisation's infrastructure, including networks, servers, and other critical systems. Their knowledge of network infrastructure, system configurations and application integrations is essential for effective threat modelling.|
|**Governance, Risk and Compliance Team**|The GRC team is responsible for organisation-wide compliance assessments based on industry regulations and internal policies. They collaborate with the security team to align threat modelling with the organisation's risk management objectives.|
|**Business Stakeholders**|The business stakeholders provide valuable input on the organisation's critical assets, business processes, and risk tolerance. Their involvement ensures that the efforts align with the organisation's strategic goals.|
|**End Users**|As direct users of a system or application, end users can provide unique insights and perspectives that other teams may not have, enabling the identification of vulnerabilities and risks specific to user interactions and behaviours.|



### Attack Trees
Creating attack tree's is another good way to identify and map threats.

An attack tree is a graphical representation used in threat modeling to systematically describe and analyse potential threats against a system, application or infrestructure. It provides a sutructed approach to breaking down scenarios into smaller components. Each node in the tree represents a specific event or condition.

![[Pasted image 20231218011852.png]]



# Modeling With MITRE ATT&CK
MITRE ATT&CK - Adversarial Tactics, Techniques, and Common Knowledge)
- Comprehensive global knowledge base of cyber adversary TTP's.
- Valuable for understanding the different stages of cyber attacks and develop effective defences

1. Technique name and Details
	- Info such as:
		- Name, explanation of the technique, types of data or logs that can help or detect, and platforms (OS) relevancy
2. Procedure Examples
	- Real-World examples of how threat actors have employed the technique in their adversarial operations
3. Mitigations
	- Recommended security measures and best practices to protect against the technique mention. I.e., sandboxing, Network segmentation, Account Management, updating etc.
4. Detections
	- Strategies and idicators that can help identify the technique, as well as potential challenges in detecting the technique
5. References
	- External sources, reports, and articles that provide additional information, context, or examples related to the technique




Searching and Selecting Techniques

﻿The first important feature we want to utilise in this application is the search functionality under the **Selection Controls** panel. This feature allows us to search and multi-select techniques you want to highlight or mark.

You may press the magnifier button to access the right sidebar, search using any keywords, or choose any selection under **Techniques,** **Threat Groups, Software, Mitigations, Campaigns, or Data Sources**.

![[Pasted image 20231218021258.png]]


Viewing, Sorting and Filtering Layers

We will tackle the next set of features under the **Layer Controls** panel. However, we will only focus on the following:

- Exporting features (download as JSON, Excel, SVG) - This allows you to dump the selected techniques, including all annotations. The data exported can be ingested again in the ATT&CK Navigator for future use.

- Filters - Allows you to filter techniques based on relevant platforms, such as operating systems or applications. For a quick example, the image below shows all techniques that are attributed to Office365.

![Filtered list of techniques with O365 filter.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5dbea226085ab6182a2ee0f7/room-content/4ecfb35e8b4b7784e55d46e0e419980f.png)  

- Sorting - This allows you to sort the techniques by their alphabetical arrangement or numerical scores. The image below shows that all techniques are arranged alphabetically.

![Example usage of sorting the techniques.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5dbea226085ab6182a2ee0f7/room-content/4c4b5706ce7f8c6cefead64434592760.png)  

- Expand sub-techniques - View all underlying sub-techniques under each technique, expanding the view for all techniques. You will have a similar view with the image below once you have expanded the sub-techniques.

![Expanding all sub-techniques in one view.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5dbea226085ab6182a2ee0f7/room-content/57b9e0faedfe675aacbb7e039c2347bb.png)  

Annotating Techniques

﻿Now, the last set of features is under the **Technique Controls** panel. These buttons allow you to annotate details on selected techniques. For a quick run-through, here are the features under the panel mentioned (from left to right):

![Buttons under the technique controls.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5dbea226085ab6182a2ee0f7/room-content/873f7f63e9ccdeca22cba95214f21179.png)  

- Toggle state - This feature allows you to disable the selected techniques, making their view greyed-out.


- Background color - This allows you to change the background color of the selected technique, for highlighting and grouping purposes.

![Example usage of changing the background colours of the techniques.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5dbea226085ab6182a2ee0f7/room-content/e319c6fbf48e55db4295ffe97a2bc99c.png)  

- Scoring - Allows you to rate each technique or set of techniques based on criteria depending on your needs, such as the impact of a technique.
- Comment - Allows you to add notes and observations to a technique.
- Link - Allows you to add external links, such as additional references related to the technique.
- Metadata - Allows you to add custom tags and labels to a particular technique.
- Clear annotations on selected - Remove all annotations on selected techniques.


**Critical Asssets** that are handled based on business stakeholders are the following:
- Customer financial data
- Transaction Records
- personally Identifiable Information (PII)

Techniques worth considering:
- Exploit Public-Facing Applications (T1190)
- Exploitation for Privilege Escalation (T1068)
- Data from Cloud Storage (T1530)
- Network Denial of Service (T1498)



# DREAD Framework
Risk assessment model developed by Microsoft to evaluate and prioritise security threats and vulnerabilities. It's an acronym that stands for:

|**DREAD**|**Definition**|
|:-:|:-:|
|**Damage**|The potential harm that could result from the successful exploitation of a vulnerability. This includes data loss, system downtime, or reputational damage.|
|**Reproducibility**|The ease with which an attacker can successfully recreate the exploitation of a vulnerability. A higher reproducibility score suggests that the vulnerability is straightforward to abuse, posing a greater risk.|
|**Exploitability**|The difficulty level involved in exploiting the vulnerability considering factors such as technical skills required, availability of tools or exploits, and the amount of time it would take to exploit the vulnerability successfully.|
|**Affected Users**|The number or portion of users impacted once the vulnerability has been exploited.|
|**Discoverability**|The ease with which an attacker can find and identify the vulnerability considering whether it is publicly known or how difficult it is to discover based on the exposure of the assets (publicly reachable or in a regulated environment).|



Damage - How bad?
Reproducibility - How easy is it to reproduce the attack?
Exploitability - How much work is it to launch the attack?
Affected Users - How many people will be impacted?
Discoverability - How easy is it to discover the vulnerability?


1. Unauthenticated Remote Code Execution (Score: 8)
    
    - Damage (D): **10**
    - Reproducibility (R): **7.5**
    - Exploitability (E): **10**
    - Affected Users (A): **10**
    - Discoverability (D): **2.5**
    
2. Insecure Direct Object References (IDOR) in User Profiles (Score: 6.5)
    
    - Damage (D): **2.5**
    - Reproducibility (R): **7.5** 
    - Exploitability (E): **7.5**
    - Affected Users (A): **10** 
    - Discoverability (D): **5**
    
3. Server Misconfiguration Leading to Information Disclosure (Score: 5)
    
    - Damage (D): **0**
    - Reproducibility (R): **10**
    - Exploitability (E): **10**
    - Affected Users (A): **0**
    - Discoverability (D): **5**

---

# STRIDE Framework
Developed by microsoft.
Helps identify and categorise potential security threats in software development and system design. STRIDE is based on 6 categories of threats:
- Spoofing - Unauthorised access or impersonation of a user or system.
- Tampering - Unauthorised modification or manipulation of data or code.
- Repudiation - Ability to deny having acted, typically due to insufficient auditing or logging 
- Information disclosure
- Denial of Service
- Elevation of priv's

- Spoofing 
    - Sending an email as another user.
    - Creating a phishing website mimicking a legitimate one to harvest user credentials.
- Tampering
    - Updating the password of another user.
    - Installing system-wide backdoors using an elevated access.
- Repudiation
    - Denying unauthorised money-transfer transactions, wherein the system lacks auditing.
    - Denying sending an offensive message to another person, wherein the person lacks proof of receiving one.
- Information Disclosure 
    - Unauthenticated access to a misconfigured database that contains sensitive customer information.
    - Accessing public cloud storage that handles sensitive documents.
- Denial of Service  
    - Flooding a web server with many requests, overwhelming its resources, and making it unavailable to legitimate users.
    - Deploying a ransomware that encrypts all system data that prevents other systems from accessing the resources the compromised server needs.
- Elevation of Privilege  
    - Creating a regular user but being able to access the administrator console.
    - Gaining local administrator privileges on a machine by abusing unpatched systems.


|                                                                                                                                                           |              |               |                 |                            |                       |                            |
| --------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | ------------- | --------------- | -------------------------- | --------------------- | -------------------------- |
| **Scenario**                                                                                                                                              | **Spoofing** | **Tampering** | **Repudiation** | **Information Disclosure** | **Denial of Service** | **Elevation of Privilege** |
| **Sending a spoofed email, wherein the mail gateway lacks email security and logging configuration.**                                                     | ✔            |               | ✔               |                            |                       |                            |
| **Flooding a web server with many requests that lack load-balancing capabilities.**                                                                       |              |               |                 |                            | ✔                     |                            |
| **Abusing an SQL injection vulnerability.**                                                                                                               |              | ✔             |                 | ✔                          |                       |                            |
| **Accessing public cloud storage (such as AWS S3 bucket or Azure blob) that handles customer data**.                                                      |              |               |                 | ✔                          |                       |                            |
| **Exploiting a local privilege escalation vulnerability due to the lack of system updates and modifying system configuration for a persistent backdoor.** |              | ✔             |                 |                            |                       | ✔                          |


## Threat Modeling with STRIDE
It's essential to integrate the six threat categories into a systematic process that effectively identifies, assesses, and mitigates security risks. Below is a high-level approach to incorporating STRIDE in the threat modeling methodologies we discussed

1. **System Decomposition**
- Break down all accounted systems into components, such as applications, networks, and data flows. Understand the architecture, trust boundaries, and potential attack surfaces
2. **Apply STRIDE Categories**
- For each component, analyze its exposure to the six STRIDE threat categories. Identify potential threats and vulnerabilities related to each category
3. **Threat Assessment**
- Evaluate the impact and likelihood of each identified threat. Consider the potential consequences and the ease of exploitation and prioritise threats based on their overall risk level
4. **Develop Countermeasures**
- Design and implement security controls to address the identified threats tailored to each STRIDE category. For example, to enhance email security and mitigate spoofing threats, implement DMARC, DKIM, and SPF, which are email authentication and validation mechanisms that help prevent email spoofing, phishing, and spamming
5. **Validation and Verification**
- Test the effectiveness of the implemented countermeasures to ensure they effectively mitigate the identified threats. If possible, conduct penetration testing, code reviews, or security audits
6. **Continuous Improvements**
- Regularly review and update the threat model as the system evolves and new threats emerge. Monitor the effective countermeasures and update them as needed
###### Skill Assessment
Scenario: Your e-commerce company is in the process of designing a new payment processing system. To ensure its security and minimise the risk of compromise, you are tasked to conduct a threat modelling exercise using the STRIDE framework. All your assets are stored in a secure cloud infrastructure developed by your system architects.

As a guide, here are the roles and responsibilities of the teams joining the initiative:

| *Team*                          | *Roles and Responsibilities*                                                                                                                            |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Development Team**            | Responsible for building systems and applications used by the organisation.                                                                             |
| **System Architecture Team**    | Responsible for designing the overall architecture of the cloud services used by the organisation.                                                      |
| **Security Team**               | Provide expertise on threats, vulnerabilities, and risk mitigation strategies.                                                                          |
| **Business Stakeholder Team**   | Provides valuable input on critical assets and business processes, and ensures alignment between the initiative and the organisation's strategic goals. |
| **Network Infrastructure Team** | Manages the organisation's network infrastructure, including servers and critical systems.                                                              |

**Software Development:**
- Registration and Authentication
- User Dashboard
- Search Functionality
- Payment

**System Architect**
- Amazon EC2 (Elastic Compute Cloud)
- RDS (Relational Database Service)
- Amazon S3

**Security Engineer**
*Common External attacks to be cognisant of:*
- DDoS
- SQL Injection Attacks
- Brute-Forcing of credentials
- Enumeration of exposed assets

**Strategic Planning**
- personal and financial data protection
- Secure our transaction processing systems
- Ensure the availability and integrity of our platform

**Network Engineer**
- Database servers
- Firewalls
- Mail servers
- File Servers

---

# PASTA Framework
**PASTA** - Process for Attack Simulation and Threat Analysis
- Structured, risk-centric threat modeling framework designed to help organizations identify and evaluate security threats and vulnerablities
- Provides a systematic, 7-step process that enables security teams to understand potential attack scenarios better, assess the likelihood and impact of threats, and prioritise remediation efforts accordingly


### Seven-Step Methodology
Similar to the high-level process discussed in the previous task, the PASTA framework covers a series of steps, from defining the scope of the threat modelling exercise to risk and impact analysis. Below is an overview of the seven-step methodology of the PASTA Framework.

![The 7-step PASTA methodology diagram.](https://tryhackme-images.s3.amazonaws.com/user-uploads/5dbea226085ab6182a2ee0f7/room-content/ae0317162059a8fcf5730e38d3853e01.svg)

1. **Define the Objectives**
    
    Establish the scope of the threat modelling exercise by identifying the systems, applications, or networks being analysed and the specific security objectives and compliance requirements to be met.
    
2. **Define the Technical Scope**
    
    Create an inventory of assets, such as hardware, software, and data, and develop a clear understanding of the system's architecture, dependencies, and data flows.
    
3. **Decompose the Application**
    
    Break down the system into its components, identifying entry points, trust boundaries, and potential attack surfaces. This step also includes mapping out data flows and understanding user roles and privileges within the system.
    
4. **Analyze the Threats** 
    
    Identify potential threats to the system by considering various threat sources, such as external attackers, insider threats, and accidental exposures. This step often involves leveraging industry-standard threat classification frameworks or attack libraries.
    
5. **Vulnerabilities and Weaknesses Analysis**  
    
    Analyze the system for existing vulnerabilities, such as misconfigurations, software bugs, or unpatched systems, that an attacker could exploit to achieve their objectives. Vulnerability assessment tools and techniques, such as static and dynamic code analysis or penetration testing, can be employed during this step.
    
6. **Analyze the Attacks**  
    
    Simulate potential attack scenarios and evaluate the likelihood and impact of each threat. This step helps determine the risk level associated with each identified threat, allowing security teams to prioritize the most significant risks.
    
7. **Risk and Impact Analysis**  
    
    Develop and implement appropriate security controls and countermeasures to address the identified risks, such as updating software, applying patches, or implementing access controls. The chosen countermeasures should be aligned with the organisation's risk tolerance and security objectives.


PASTA Methodology Guidelines  

To effectively implement the PASTA framework and optimise its benefits, you may follow these practical guidelines for each step of the methodology.

| **Define the Objectives**                   | - Set clear and realistic security objectives for the threat modelling exercise.<br>- Identify relevant compliance requirements and industry-specific security standards.                                                                                                                |
| ------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Define the Technical Scope**              | - Identify all critical assets, such as systems and applications, that handle sensitive data owned by the organisation.<br>- Develop a thorough understanding of the system architecture, including data flows and dependencies.                                                         |
| **Decompose the Application**               | - Break down the system into manageable components or modules.<br>- Identify and document each component's possible entry points, trust boundaries, attack surfaces, data flows, and user flows.                                                                                         |
| **Analyse the Threats**                     | - Research and list potential threats from various sources, such as external attackers, insider threats, and accidental exposures.<br>- Leverage threat intelligence feeds and industry best practices to stay updated on emerging threats.                                              |
| **Vulnerabilities and Weaknesses Analysis** | - Use a combination of tools and techniques, such as static and dynamic code analysis, vulnerability scanning, and penetration testing, to identify potential weaknesses in the system.<br>- Keep track of known vulnerabilities and ensure they are addressed promptly.                 |
| **Analyse the Attacks**                     | - Develop realistic attack scenarios and simulate them to evaluate their potential consequences.<br>- Create a blueprint of scenarios via Attack Trees and ensure that all use cases are covered and aligned with the objective of the exercise.                                         |
| **Risk and Impact Analysis**                | - Assess the likelihood and impact of each identified threat and prioritise risks based on their overall severity.<br>- Determine the most effective and cost-efficient countermeasures for the identified risks, considering the organisation's risk tolerance and security objectives. |

### Application of PASTA Framework
Scenario: Your organization is known for it's online banking platform, catering to many users across the Asia Pacific region. To ensure its resiliency to potential threats, you are tasked to conduct a threat modeling exercise using the PASTA framework.

| **Team**                      | **Roles and Responsibilities**                                                                                                                          |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Development Team**          | Responsible for building systems and applications used by the organisation.                                                                             |
| **System Architecture Team**  | Responsible for designing the overall architecture of the cloud services used by the organisation.                                                      |
| **Security Team**             | Provide expertise on threats, vulnerabilities, and risk mitigation strategies.                                                                          |
| **Business Stakeholder Team** | Provides valuable input on critical assets and business processes, and ensures alignment between the initiative and the organisation's strategic goals. |

In which step of the framework do you break down the system into its components?
> `Decompose the application`

During which step of the PASTA Framework do you simulate potential attack scenarios?
> `Analyse the attacks`

In which step of the PASTA framework do you create an inventory of assets?
> `Define the technical scope`


1. Strategic Planning
- Top priorities: Customer's personal and financial data, secure our transactions processing systems, and ensure the availability and integrity of our online banking services

2. System Architecture
- Leverages several AWS services to support its functionality
- EC2 (Elastic Compute cloud)
- RDS (relational Database Service)
- Amazon S3
- Load Balancer
**Data Flows**: Utilize amazon S3 for storing static assets and files, such as customer profile pictures and document uploads, EC2 for hosting microservices via Virtual Machines, and RDS for storing customer data

3. Software Development
- Secure user registration and authentication
- Account Management, Fund transfers, bill payments, and account statements
- Mobile app for convenient access to our services

4. Information Security
- External attackers can try to break into our system via different methods such as brute-force attacks, SQL Injection, Cross-site scripting, or distributed denial of service (DDoS)
- Insecure AWS Configurations, exposed public buckets and RDS endpoints, may incur potential risks
*Solutions:*
- Implement account lockouts after several failed attempts for brute-forcing attacks
- SQL Injections can be mitigated by using secure coding practices
- For XSS: Ensure data output is correctly encoded. 
- For DDoS Attacks: use services that automatically protect against such threats and plan for scaling to handle the increased load.
- For Cloud Security: Harden all AWS services and limit the accessibility of each one only to the intended data flows
#### Business Analyst

Well, these attacks could have severe consequences. A successful attack could lead to financial loss for our customers and our bank, regulatory penalties, and significant reputational damage.
#### Network Engineer

Sure, we maintain servers such as:

- Database servers
- Firewalls
- Mail servers
- File servers




| **Framework**  | **Use Case Applications**                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `MITRE ATT&CK` | Unlike DREAD and STRIDE, which focus more on potential risks and vulnerabilities, ATT&CK provides a practical and hands-on approach by mapping adversary tactics.<br><br>- Assess the effectiveness of existing controls against known attack techniques used by threat actors.                                                                                                                                                                                                      |
| `DREAD`        | DREAD offers a more numerical and calculated approach to threat analysis than STRIDE or MITRE ATT&CK, making it excellent for clearly prioritising threats.  <br><br>- Qualitatively assess the potential risks associated with specific threats.<br>- Prioritise risk mitigation based on the collective score produced by each DREAD component.                                                                                                                                    |
| `STRIDE`       | While other frameworks like MITRE ATT&CK focus on real-world adversary tactics, STRIDE shines in its structure and methodology, allowing for a systematic review of threats specific to software systems.  <br><br>- Analyse and categorise threats in software systems.<br>- Identify potential vulnerabilities in system components based on the six STRIDE threat categories.<br>- Implement appropriate security controls to mitigate specific threat types.                     |
| `PASTA`        | Excellent for aligning threat modelling with business objectives. Unlike other frameworks, PASTA integrates business context, making it a more holistic and adaptable choice for organisations.  <br><br>- Conduct risk-centric threat modelling exercises aligned with business objectives.<br>- Prioritise threats based on their potential impact and risk level to the organisation.<br>- Build a flexible methodology that can be adapted to different organisational contexts. |







































