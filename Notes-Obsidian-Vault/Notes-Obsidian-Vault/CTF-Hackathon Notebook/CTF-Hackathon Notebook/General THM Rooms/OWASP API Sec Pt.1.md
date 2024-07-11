## Table of Contents

- [\#1 Broken Object Level Authorisation (BOLA)](#\#1\broken\object\level\authorisation\(bola))
- [\#2 Broken User AUthentication](#\#2\broken\user\authentication)
- [\# 3 Excessive Data Exposure](#\#\3\excessive\data\exposure)
- [\# 4 Lack of Resources and Rate Limiting](#\#\4\lack\of\resources\and\rate\limiting)
- [\# 5 Broken Function Level Authorisation](#\#\5\broken\function\level\authorisation)
      - [Questions & Answers](#Questions\&\Answers)

# \#1 Broken Object Level Authorisation (BOLA)
**Absence of controls to prevent unauthorised object access can lead to data leakage and in some cases complete account takeover.
	- Refers to IDOR (insecure Direct Object Reference) - user use the input functionality and get access to the resources they are not authorised to**


# \#2 Broken User AUthentication
**Happens from not  implementing a strong password policy and not correctly verifying the username/password correctly/authtoken**

# \# 3 Excessive Data Exposure
**Exposing more data than need be**

# \# 4 Lack of Resources and Rate Limiting
- **No captchas**
- **No restrictions on client requests of the file sizes'**

# \# 5 Broken Function Level Authorisation
**When low priv user bypasses system checks and gets access to confidential data by impersonation. 
- Access to bad shit thru hidden header/body/data values


















#### Questions & Answers
Can Rate limiting be carried out at the network level through firewall?
**Yes**








