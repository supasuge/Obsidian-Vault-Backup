## Table of Contents


Understanding Security Posture

﻿Like with every new process, understanding your gaps and state is critical for successfully introducing a new tool, solution, or change. To help grasp what your security posture is, you can start by doing the following:﻿

- Perform a **gap analysis** to determine what activities and policies exist in your organisation and how effective they are. For example, ensuring policies are in place (what the team does) with security procedures (how the team executes those policies).  
    
- Create **Software Security Initiatives** (SSI) by establishing realistic and achievable goals with defined metrics for success. For example, this could be a Secure Coding Standard, playbooks for handling data, etcetera are tracked using project management tools.  
    
- **Formalise processes** for security activities within your SSI. After starting a program or standard, it is essential to spend an operational period helping engineers get familiarised with it and gather feedback before enforcing it. When performing a gap analysis, every policy should have defined procedures to make them effective.  
    
- **Invest in security training** for engineers as well as appropriate tools. Ensure people are aware of new processes and the tools that will come with them to operationalise them, and invest in training early, ideally before establishing / onboarding the tool.

**SSDLC Processes**

After understanding your security posture, now is the time to prioritise and instil security in your SDLC. Generally speaking, a secure SDLC involves integrating processes like security testing and other activities into an existing development process. Examples include writing security requirements alongside functional requirements and performing an architecture risk analysis during the design phase of the SDLC. These processes are the following:

![Risk Matrix](https://tryhackme-images.s3.amazonaws.com/user-uploads/61a7523c029d1c004fac97b3/room-content/3d1e3379a1e3ccc46f3c7471095cbfae.png) 

- **Risk Assessment** - during the early stages of SDLC, it is essential to identify security considerations that promote a security by design approach when functional requirements are gathered in the planning and requirements stages. For example, if a user requests a blog entry from a site, the user should not be able to edit the blog or remove unnecessary input fields.  
    
- **Threat Modelling** - is the process of identifying potential threats when there is a lack of appropriate safeguards. It is very effective when following a risk assessment and during the design stage of the SDLC, as Threat Modelling focuses on what should not happen. In contrast, design requirements state how the software will behave and interact. For example, ensure there is verification when a user requests account information.
- **Code Scanning / Review** -  Code reviews can be either manual or automated. Code Scanning or automated code reviews can leverage Static and Dynamic Security testing technologies. These are crucial in the Development stages as code is being written.
- **Security Assessments - Like Penetration Testing & Vulnerability Assessments** are a form of automated testing that can identify critical paths of an application that may lead to exploitation of a vulnerability. However, these are hypothetical as the assessment doesn't carry simulations of those attacks. Pentesting, on the other hand, identifies these flaws and attempts to exploit them to demonstrate validity. Pentests and Vulnerability Assessments are carried out during the Operations & Maintenance phase of the SDLC after a prototype of the application.

There are methodologies to apply the processes in task 5 that will help you navigate and guide you when introducing risk assessment, threat modelling, scanning and testing, and operational assurance. The following tasks will cover these processes in more detail.


﻿﻿Risk Assessment

Risk refers to the likelihood of a threat being exploited, negatively impacting a resource or the target it affects. For example, vulnerabilities being exploited after a new version of the software is published, design flaws, and poorly reviewed code can increase the risk of these scenarios. Risk management is an important pillar to integrate into the SDLC to mitigate risk in a product or service.  

---

Risk assessment is used to determine the level of the potential threat. Risk identified in the risk assessment process can be reduced or eliminated by applying appropriate controls during the risk mitigation process. Usually, a risk assessment is followed by threat modelling, which will be explained further in the next section.  

**Performing a Risk Assessment**

1. The first step in the risk assessment process is to assume the software will be attacked and consider the factors that motivate the threat actor. List out the factors such as the data value of the program, the security level of companies who provide resources that the code depends on, the clients purchasing the software, and how big is the software distributed (single, small workgroup or released worldwide). Based on these factors, write down the acceptable level of risk. For instance, a data loss may cause the company to lose millions, especially if they require to pay fines nowadays with GDPR, but eliminating all potential security bugs in the code may cost $40,000. The company and some other stakeholder groups have to decide whether it is worth it; it is also crucial to communicate these tradeoffs. Hence, everyone has an understanding of risk and its implications. From a brand reputation perspective, if the attack causes damage to the company's image, it costs the company more in the long run than fixing the code.
2. The next step is risk evaluation. Include factors like the worst-case scenario if the attacker has successfully attacked the software. You can demonstrate the worst-case scenario to executives and senior engineers by simulating a ransomware attack. Determine the value of data to be stolen, valuable data such as the user's identity, credentials to gain control of the endpoints on the network, and data or assets that pose a lower risk. Another factor to consider is the difficulty of mounting a successful attack, in other words, its complexity. For example, if an attacker can gain access to the company's tool for giving feedback to colleagues or running retrospective meetings, it would have a lower impact than accessing a production environment's monitoring and alerts system. The high level of risk will not be acceptable, and it is best to mitigate it. For example, a vulnerability can be exploited by anyone running prewritten attack scripts or using botnets to spread the scripts to compromise computers and networks. Users affected are a vital factor.
3. Some attacks only affect one or two users, but the denial of service attack will affect thousands of users when a server is attacked. Moreover, thousands of computers may be infected by the spread of worms. The last factor is the accessibility of the target. Determine whether the target accepts requests across a network or only local access, whether authentication is needed for establishing a connection, or if anyone can send requests. It has more impact if an attacker accesses a production environment vs a sandbox environment used in local playgrounds for people to do labs or tutorials.

  
![Risk Matrix](https://tryhackme-images.s3.amazonaws.com/user-uploads/61a7523c029d1c004fac97b3/room-content/e5fb607dfd764c3e162da1783dd58e02.png)

  

  

  

Risk assessments are better performed at the beginning of the SDLC, during the planning and requirement phases. For example, "Customer data gets exfiltrated by an attack". Once the system is developed, you can perform quantitative risk analysis: "One customer can sue us for $ 20,000 if their data gets leaked", and we have 100 customers. However, the Annual Rate of Occurrence (ARO) is 0.001. Hence Annual Loss Expectancy is = $20,000 * 100 * 0.001 = $ 2,000, meaning as long as our compensating security control is less than $ 2,000, we are not overspending on security.



Threat Modelling

Threat modelling is best integrated into the design phase of an SDLC before any code is written. Threat modelling is a structured process of identifying potential security threats and prioritising techniques to mitigate attacks so that data or assets that have been classified as valuable or of higher risk during risk assessment, such as confidential data, are protected. When performed early, it brings a great advantage; potential issues can be found early and solved, saving fixing costs down the line. 

  

There are various methods to perform threat modelling. Not all the methods have the same purpose; some focus on risk or privacy concerns, while some are more customer-focused. We can combine these methods to understand potential threats better; it is essential to analyse which way aligns more with the project or business. STRIDE, DREAD, and PASTA are among the common threat modelling methodologies.

the project or business. STRIDE, DREAD, and PASTA are among the common threat modelling methodologies.  

**STRIDE**  

STRIDE stands for Spoofing, Tampering, Repudiation, Information Disclosure, Denial Of Service, and Elevation/Escalation of Privilege. STRIDE is a widely used threat model developed by Microsoft which evaluates the system's design in a more detailed view. We can use STRIDE to identify threats, including the property violated by the threat and definition. The system's data flow diagram is to be developed in this model, and each node is applied with the STRIDE model. Identifying security threats is a manual process that tools are not supported and should be carried out during the risk assessment. Using data flow diagrams and integrating STRIDE, the system entities, attack surfaces, like known boundaries and attacker events become more identifiable. Stride stands for Spoofing Identity, Tampering with Data, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege. STRIDE is built upon the CIA triad principle (Confidentiality, Integrity & Availability). Security professionals that perform STRIDE are looking to answer "What could go wrong with this system". Here are the components of the framework:

![STRIDE diagram](https://tryhackme-images.s3.amazonaws.com/user-uploads/61a7523c029d1c004fac97b3/room-content/a5904cf6f063a62e77df6623eb1cbd33.png)

1. **Spoofing:** It is an act of impersonation of a user by a malicious actor which violates the authentication principle from the perspective of the CIA triad. Common ways include ARP, IP, and DNS spoofing.
    
2. **Tampering:** The modification of information by an unauthorised user. It violates the integrity principle of the CIA triad.
    
3. **Repudiation:** Not taking responsibility for events where the actions are not attributed to the attacker. It violates the principle of non-repudiability. For example, the attacker clears up all the logs that could lead to leaving traces.
    
4. **Information Disclosure:** It is an act of violation of confidentiality of the CIA triad. A typical example is data breaches.
    
5. **Denial of Service:** Denial of Service occurs when an authorised user cannot access the service, assets, or system due to the exhaustion of network resources. DoS is a violation of the availability principle of the CIA triad.
    
6. **Elevation/Escalation of Privilege:** Escalating privileges to gain unauthorised access is a classic example of a violation of the authorisation principle of the CIA triad.

**DREAD**

The abbreviation DREAD stands for five questions about each potential: Damage Potential, Reproducibility, Exploitability, Affected Users and Discoverability. DREAD is also a methodology created by Microsoft which can be an add-on to the STRIDE model. It's a model that ranks threats by assigning identified threats according to their severity and priority. In other words, it creates a rating system that is scored based on risk probability. Without STRIDE, the DREAD model also can be used in assessing, analysing and finding the risk probability by threat rating. Here are the components of the framework:

1. **Damage:** refers to the possible damage a threat could cause to the existing infrastructure or assets. It is based on a scale of 0–10. A score of 0 means no harm, 5 means Information Disclosure, 8 means user data is compromised, 9 means internal or administrative data is compromised, and 10 means unavailability of a service.
    
2. **Reproducibility:** it measures the complexity of the attack. So how easily a hacker can replicate a threat. A score of 0 means it is nearly impossible to copy, 5 stands for being complex but possible, 7.5 for an authenticated user and a score of 10 means the attacker can reproduce very quickly without any authentication.
    
3. **Exploitability:** Referring to the attack's sophistication or how easy it would be to launch the attack. A score of 2.5 implies it would require an advanced skill set of networking and programming skills; 5 means can be exploited with available tools, a score of 9 means we would need a simple web application proxy tool and a score of 10 means it can exploit through a web browser.
    
4. **Affected Users:** it describes the number of users affected by the successful exploitation of a vulnerability. A score of 0 would mean that there would be no affected users, 2.5 shall mean for an individual user, 6 would mean a small group of users, 9 would mean significant users like administrative users, and 10 would imply all users are affected.
    
5. **Discoverability:** The process of discovering the vulnerable points in the system. For example, the threat would be easily found in case of compromise. A score of 0 would mean it would be challenging to discover it, a score of 5 means that the threat can be discovered by analysis of HTTP requests, and 8 means it can be easily found as it's public-facing. A score of 10 would mean it's visible in the browser address bar.


The abbreviation DREAD stands for five questions about each potential: Damage Potential, Reproducibility, Exploitability, Affected Users and Discoverability. DREAD is also a methodology created by Microsoft which can be an add-on to the STRIDE model. It's a model that ranks threats by assigning identified threats according to their severity and priority. In other words, it creates a rating system that is scored based on risk probability. Without STRIDE, the DREAD model also can be used in assessing, analysing and finding the risk probability by threat rating. Here are the components of the framework:

1. **Damage:** refers to the possible damage a threat could cause to the existing infrastructure or assets. It is based on a scale of 0–10. A score of 0 means no harm, 5 means Information Disclosure, 8 means user data is compromised, 9 means internal or administrative data is compromised, and 10 means unavailability of a service.
    
2. **Reproducibility:** it measures the complexity of the attack. So how easily a hacker can replicate a threat. A score of 0 means it is nearly impossible to copy, 5 stands for being complex but possible, 7.5 for an authenticated user and a score of 10 means the attacker can reproduce very quickly without any authentication.
    
3. **Exploitability:** Referring to the attack's sophistication or how easy it would be to launch the attack. A score of 2.5 implies it would require an advanced skill set of networking and programming skills; 5 means can be exploited with available tools, a score of 9 means we would need a simple web application proxy tool and a score of 10 means it can exploit through a web browser.
    
4. **Affected Users:** it describes the number of users affected by the successful exploitation of a vulnerability. A score of 0 would mean that there would be no affected users, 2.5 shall mean for an individual user, 6 would mean a small group of users, 9 would mean significant users like administrative users, and 10 would imply all users are affected.
    
5. **Discoverability:** The process of discovering the vulnerable points in the system. For example, the threat would be easily found in case of compromise. A score of 0 would mean it would be challenging to discover it, a score of 5 means that the threat can be discovered by analysis of HTTP requests, and 8 means it can be easily found as it's public-facing. A score of 10 would mean it's visible in the browser address bar.
    

**PASTA**

PASTA is short for Process for Attack Simulation and Threat Analysis; it is a risk-centric threat modelling framework. PASTA's focus is to align technical requirements with business objectives. PASTA involves the threat modelling process from analysing threats to finding ways to mitigate them, but on a more strategic level and from an attacker's perspective. It identifies the threat, enumerates them, and then assigns them a score. This helps organisations find suitable countermeasures to be deployed to mitigate security threats. PASTA is divided into seven stages:

![PASTA diagram](https://tryhackme-images.s3.amazonaws.com/user-uploads/61a7523c029d1c004fac97b3/room-content/8222ed08e31753d975ec3ec3aa2e43a9.png)  

1. **Define Objectives:** The first stage focuses on noting the structure and defining objectives. This makes the end goal a whole lot clearer and ensures the relevant assets are threat modelled by defining an asset scope.
    
2. **Define Technical Scope:** This is where architectural diagrams are defined, both logical and physical infrastructure. Helpful in mapping the attack surface and dependencies from the environment.
    
3. **Decomposition & Analysis:** Each asset will have a defined trust boundary that encompasses all its components and data in this stage. For example, mapping threat vectors for a payment service and evaluating which components underlying the service can be leveraged for an attack; components can be libraries, dependencies, modules or underlying services etc.
    
4. **Threat Analysis:** This refers to the extracted information obtained from threat intelligence. Useful to identify which applications are vulnerable to specific vectors; for example, a customer/public-facing application can be susceptible to DDOS, unauthorised data alteration etc.
    
5. **Vulnerabilities & Weaknesses Analysis:** it analyses the vulnerabilities of web application security controls. It identifies security flaws in the application and enumerates vulnerabilities. It is highly recommended to add mitigation to the identified threat in this stage. For example, when describing a past incident involving an exploit of a mail server, lessons learned or mitigation: lack of thorough testing before implementation and hardening the server.
    
6. **Attack/Exploit Enumeration & Modelling:** In this stage, we map out the possible threat landscape and the entire attack surface of our web application. We then map all potential attack vectors to the different nodes, identifying exploits and attack paths. This stage simulates all the enumerated information extracted from all of the previous steps; this helps security professionals determine the extent and probability of successfully launching the identified vulnerabilities.
    
7. **Risk Impact Analysis:** Based on the collective data from the previous stages, all of the scoped assets that have been affected are analysed and finally, based upon the risk analysis, recommended steps to mitigate the risks and eliminate all of the residual risks are documented.


﻿﻿﻿﻿Secure code review & analysis

According to research conducted by Verizon in 2020 on Data Breaches, 43% of breaches were attacks on web applications, while some other security breaches resulted from some vulnerabilities in web applications. Implementing a secure code review in the phases of an SDLC, especially during the implementation phase, will increase the resilience and security of the product without bearing any additional cost for future patches. Secure code review is defined as a measure where the code itself is verified and validated to ensure vulnerabilities that are found can be mitigated and removed to avoid vulnerabilities and flaws. Having the developers be aware and proactive in reviewing the code during development can result in faster mitigation responses and fewer unattended threats. Reviewing code is a crucial step in the SDLC for developers. It prevents any setbacks on the release and issues exposed to the users. As shown in the previous SSDLC task, the cost of the project itself and the effort put in is proportional and cheaper in the long run than the cost of applying code reviews and analysis. By implementing this approach, the organisation will also be compliant with the standards set by government bodies and certifications. 

  

Code review can be done manually or automated. A manual code review is where an expert analyses and checks the source code by going line by line to identify vulnerabilities. Therefore, a high-quality manual code review requires the expert to communicate with the software developers to get hold of the purpose and functionalities of the application. The analysis output will then be reported to the developers if there is a need for bug fixing.   

  

Code Analysis

Static analysis examines the source code without executing the program. In contrast, Dynamic analysis looks at the source

code when the program is running, static analysis detects bugs at the implementation level, and dynamic analysis detects errors during program runtime. Automated Static Application Security Testing (SAST) automatically analyses and checks the source code.