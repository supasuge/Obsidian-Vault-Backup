## Table of Contents




Implementing a training solution:


1. Idneitfy hthe program scope goals, and objectives
2. Identify training staff
3. 


According to CERT, if the FBI is involved, investigators will gather information in four ways:
- Request for voluntary disclosure of information
- Court Order
- Federal grand jury subpoena
- Search Warrant

Each state has individual computer crime laws. There are also federal laws that can be prosecuted as well. FBI will generally investiagte the following cases telated to computer crime:
- Interstate Communications: Incluiding Threats, Kidnapping, Random, Extortion
- Possession of Access Devices
- Fraud and Related Activity in Connection with Computers
- Fraud by wire, radio or television
- Injury to government property
- Govt Infra
- Espionage (no shit)
- Trader Secrets (hint: secret)


1. Wiretap Act (18 U.S.C. 2510-22)
2. Pen Registers and Trap and Trace Devices Statute (18 U.S.C. 3121-27)
3. Stored Wire and Electronic Communication Act (18 U.S.C. 2701-120)


Security Policies focused on risk management are liekyl to be the most successful in an organization. There is a great deal of data and information available either throught NIST or through non-government organizations that offer assistance and documentation that an organization can use to effectively implement a risk management process toward the development of an organization's security policy

- **Scrub all input.** The main threat of XSS is the ability to inject code into a system in unwanted locations. Scrubbing input for unnecessary characters and altering necessary but possibly dangerous characters will go far in preventing or containing an XSS attack.
- **Escape your output for display.** The output of your system should also be monitored. In case anything illicit is allowed into your system past the scrubbers on the input, the damage should be contained by not allowing it to cause compromise as output. This involves escaping any HTML display code or scripting commands to leave the system in display or execution format. JavaScript can be employed to parse needed characters like single quotes or dashes back into names and lines of text without allowing them to cause an escape of a more sensitive process.
- **Use trusted solutions when possible.** XSS tends to cluster around known vulnerabilities, so using existing solutions with resistance to XSS or patches to prevent its use is a good practice when possible in choosing existing software solutions as part of your system. The languages used in new code production should be investigated for known vulnerabilities before they are used, just as with any other effort in programming.
- **Use a separate variable to contain scrubbed input.** Tracing variables in a web application can be less of a linear process than in other thick-client applications. Therefore, a separate variable should be used to store any input that has been scrubbed. Only the clean variable should ever be used in direct processing to avoid the case where the direct input is accidentally allowed to execute by switching the execution branching. A quick way to do this is to keep the variable names and prefix them with `dirty_` and `clean_` and allow only `clean_` versions in function calls.

**Auditing plays a major role in a Database systems security**
Always Log:
- user access attempts
- Data Control Language Activities
- Data Definition Language (DDL)
- Data Manipulation Language (DML) activities


**Overload** is a redefinition of an existing operation for a new class.
**Inheritance** is the creation of a subclass to expand functionality to serve specialized circumstances.






