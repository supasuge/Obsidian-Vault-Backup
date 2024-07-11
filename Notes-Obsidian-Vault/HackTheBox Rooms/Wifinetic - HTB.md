IP=`10.10.11.247`
Ports:
Names:
- olivia.walker17@wifinetic.htb
- samantha.wood93@wifinetic.htb

### Foothold
Using the passwords found  in the config directory, we can now try to ssh as `netadmin` and get a an initial foothold. The password/config directory was found through FTP anonymous authentication.

`echo 'netadmin:VeRyUniUqWiFIPasswrd1!' > creds`
