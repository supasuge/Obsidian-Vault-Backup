## Table of Contents

- [Uses](#uses)
        - [Table of Contents](#Table\of\Contents)
          - [Getting Started](#Getting\Started)
          - [Web Proxy](#Web\Proxy)
          - [Web Fuzzer](#Web\Fuzzer)
          - [Web Scanner](#Web\Scanner)
          - [Skills Assessment](#Skills\Assessment)
- [ProxyChains](#proxychains)
  - [Nmap](#Nmap)

# Uses
- Application vulnerability testing
- web fuzzing
- web crawling
- web application mapping
- web request analysis
- web configuration testing
- code reviews



**Note:** In case we wanted to serve the web proxy on a different port, we can do that in Burp under (`Proxy>Options`), or in ZAP under (`Tools>Options>Local Proxies`). In both cases, we must ensure that the proxy configured in Firefox uses the same port.

Another important step when using Burp Proxy/ZAP with our browser is to install the web proxy's CA Certificates. If we don't do this step, some HTTPS traffic may not get properly routed, or we may need to click `accept` every time Firefox needs to send an HTTPS request.

We can install Burp's certificate once we select Burp as our proxy in `Foxy Proxy`, by browsing to `http://burp`, and download the certificate from there by clicking on `CA Certificate`:

   

![](https://academy.hackthebox.com/storage/modules/110/burp_cert.jpg)

To get ZAP's certificate, we can go to (`Tools>Options>Dynamic SSL Certificate`), then click on `Save`:

![ZAP cert](https://academy.hackthebox.com/storage/modules/110/zap_cert.jpg)

We can also change our certificate by generating a new one with the `Generate` button.

Once we have our certificates, we can install them within Firefox by browsing to [about:preferences#privacy](about:preferences#privacy), scrolling to the bottom, and clicking `View Certificates`:

![Cert Firefox](https://academy.hackthebox.com/storage/modules/110/firefox_cert.jpg)

After that, we can select the `Authorities` tab, and then click on `import`, and select the downloaded CA certificate:

![Import Firefox Cert](https://academy.hackthebox.com/storage/modules/110/firefox_import_cert.jpg)

Finally, we must select `Trust this CA to identify websites` and `Trust this CA to identify email users`, and then click OK: ![Trust Firefox Cert](https://academy.hackthebox.com/storage/modules/110/firefox_trust_cert.jpg)

Once we install the certificate and configure the Firefox proxy, all Firefox web traffic will start routing through our web proxy.

 [Previous](https://academy.hackthebox.com/module/110/section/1046)

 Mark Complete & Next

[Next](https://academy.hackthebox.com/module/110/section/1048) 

 Cheat Sheet

##### Table of Contents

###### Getting Started

[Intro to Web Proxies](https://academy.hackthebox.com/module/110/section/1045)[Setting Up](https://academy.hackthebox.com/module/110/section/1046)

###### Web Proxy

[Proxy Setup](https://academy.hackthebox.com/module/110/section/1047)  [Intercepting Web Requests](https://academy.hackthebox.com/module/110/section/1048)[Intercepting Responses](https://academy.hackthebox.com/module/110/section/1049)[Automatic Modification](https://academy.hackthebox.com/module/110/section/1050)  [Repeating Requests](https://academy.hackthebox.com/module/110/section/1051)  [Encoding/Decoding](https://academy.hackthebox.com/module/110/section/1052)  [Proxying Tools](https://academy.hackthebox.com/module/110/section/1053)

###### Web Fuzzer

  [Burp Intruder](https://academy.hackthebox.com/module/110/section/1054)  [ZAP Fuzzer](https://academy.hackthebox.com/module/110/section/1056)

###### Web Scanner

[Burp Scanner](https://academy.hackthebox.com/module/110/section/1084)  [ZAP Scanner](https://academy.hackthebox.com/module/110/section/1086)[Extensions](https://academy.hackthebox.com/module/110/section/1104)

###### Skills Assessment


HTB{1n73rc3p73d_1n_7h3_m1ddl3}


URL Encoding--

- Spaces: Indicate end of request d ata if not encoded
- &: Interpreted as a parameter delimiter
- #: Interpreted as a fragment identifier



# ProxyChains
edit config:
/etc/proxychains.conf
uncomment quiet_mode

in the config:
#socks4 127.0.0.1 9050
http 127.0.0.1 8080

prepende each command with proxychains EX:

`proxychains curl http://MACHINE_IP`

## Nmap

Next, let's try to proxy `nmap` through our web proxy. To find out how to use the proxy configurations for any tool, we can view its manual with `man nmap`, or its help page with `nmap -h`:

```shell-session
gdxqpardo@htb[/htb]$ nmap -h | grep -i prox

  --proxies <url1,[url2],...>: Relay connections through HTTP/SOCKS4 proxies
```



































