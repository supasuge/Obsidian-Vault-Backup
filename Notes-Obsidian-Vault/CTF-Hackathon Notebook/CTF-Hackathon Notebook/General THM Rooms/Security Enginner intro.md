## Table of Contents

- [Core Responsibilities of a security engineer](#core\responsibilities\of\a\security\engineer)
    - [Secure by Design](#Secure\by\Design)
    - [Security Assessment and Assurance](#Security\Assessment\and\Assurance)
    - [Managing Security Tooling](#Managing\Security\Tooling)
    - [Tabletop Exercises](#Tabletop\Exercises)
    - [Disaster Recovery and Crisis Management](#Disaster\Recovery\and\Crisis\Management)
    - [Bell-LaPadula Model](#Bell-LaPadula\Model)


Organizations perceive a security enginner as someone who:
- Owns the overall security of an organization. the main person responsible for securing an organization's digital assets.
- Ensures that the organization's cyber security risk is minimized at all times.
- Devises strategies and creates systems that minimize the risk posed by cyber security threats to an organization.
- Periodically conducts test to ensure the robustness of the cyber security posture of an organization, identifies weak points, and prepares mitigations
- Develops and implements secure network solutions
- Architects and engineers trustworthy, reliable, and secure systems.
- Collaborates and coordinates with other teams to establish protocols across the organization.

# Core Responsibilities of a security engineer
Security Policies:
An organization needs robust security policies to maintain a sound security posture. A security engineer helps the organization create security policies based on established [Security Principles](https://tryhackme.com/room/securityprinciples). These policies are then implemented organization-wide, and the security engineer ensures that the implementation follows the letter and spirit of the policies. Sometimes, a need arises for granting exceptions to the security policies due to business needs. In such scenarios, the security engineer consults the security principles to allow or deny exceptions and suggest mitigating steps to minimize risks. 

### Secure by Design  

A security engineer ensures that the organization is secure by design. The engineer understands that the security posture receives the most Return on Investment (ROI) if it follows a secure-by-design philosophy. This means that the security engineer takes steps to implement a Secure Network Architecture _(room coming soon!)_, ensures the organization's [Windows](https://tryhackme.com/room/microsoftwindowshardening), [Linux](https://tryhackme.com/room/linuxsystemhardening), and [Active Directory](https://tryhackme.com/room/activedirectoryhardening) are hardened, and software development follows the [Secure Software Development Lifecycle](https://tryhackme.com/room/securesdlc).

### Security Assessment and Assurance

While securely designing the organization's network and infrastructure might be an excellent first step, a security engineer understands that their job is far from done after that. They understand that security is hard work that requires continuous effort. While a security engineer must ensure everything is done correctly, a compromise requires just one loophole to be successful. To mitigate risks from a continuously evolving threat landscape, a security engineer plans to conduct regular security assessments, audits, and red-teaming and purple-teaming exercises to continuously improve the security posture. While security engineers might not be performing assessments and audits themselves, they are primarily involved in helping schedule these activities, creating Request for Quotations (RFQs) for external parties to perform these activities, and helping prioritize and implement the findings from them.


### Managing Security Tooling

A security engineer might sometimes be required to configure or fine-tune security tools such as SIEMs, Firewalls, WAFs, EDRs, and more. In some organizations, that might also be the primary responsibility of a security engineer. In such a role, a security engineer might also be making decisions or providing input to decision-makers about tools to procure based on the organization's requirements and the engineer's assessments of competitive tools.

### Tabletop Exercises

Tabletop exercises are often conducted to gauge the operational readiness of an organization from a security point of view. Certain scenarios are identified to be exercised, and security team members must explain their respective roles in the scenarios under discussion. For example, a scenario might include the compromise of an endpoint device through a phishing email. All the team members will then explain their respective steps per the organization's playbooks. The security engineer is sometimes required to conduct these exercises.

### Disaster Recovery and Crisis Management

A robust security posture requires organizations to plan for untoward incidents, disasters, or crises. In any such scenario, the top priority of the executive management is to maintain business continuity. A security engineer might be involved in disaster recovery, business continuity, and crisis management planning as part of the different compliance frameworks and the organization's internal policies. The role of a security engineer in these areas might differ depending on the organization.

---

The security of a system is attacked through one of several means. It can be via the disclosure of secret data, alteration of data, or destruction of data.

- **Disclosure** is the opposite of confidentiality. In other words, disclosure of confidential data would be an attack on confidentiality.
- **Alteration** is the opposite of Integrity. For example, the integrity of a cheque is indispensable.
- **Destruction/Denial** is the opposite of Availability.

The opposite of the CIA Tria would be the DAD Triad: Disclosure, Alteration and Destruction


- Bell-LaPadula Model
- The Biba Integrity Model
- The Clark-Wilson Model

### Bell-LaPadula Model

The Bell-LaPadula Model aims to achieve **confidentiality** by specifying three rules:

- **Simple Security Property**: This property is referred to as “no read up”; it states that a subject at a lower security level cannot read an object at a higher security level. This rule prevents access to sensitive information above the authorized level.
- **Star Security Property**: This property is referred to as “no write down”; it states that a subject at a higher security level cannot write to an object at a lower security level. This rule prevents the disclosure of sensitive information to a subject of lower security level.
- **Discretionary-Security Property**: This property uses an access matrix to allow read and write operations. An example access matrix is shown in the table below and used in conjunction with the first two properties.

|Subjects|Object A|Object B|
|---|---|---|
|Subject 1|Write|No access|
|Subject 2|Read/Write|Read|



