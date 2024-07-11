## Table of Contents

- [What Info should be collected?](#What\Info\should\be\collected?)
    - [Email Header Analysis](#Email\Header\Analysis)

#phishing #analysis #blueteam #defense
### What Info should be collected?
- Sender email address
- sender IP Address
- Reverse Lookup of the sender IP address
- Email subject line
- Recipient email address
- Reply to email address
- Date/time

Below is a checklist of the artifacts an analyst (you) needs to collect from the email body:

- Any URL links (if an URL shortener service was used, then we'll need to obtain the real URL link)
- The name of the attachment
- The hash value of the attachment (hash type MD5 or SHA256, preferably the latter)


