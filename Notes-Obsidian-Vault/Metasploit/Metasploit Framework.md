## Table of Contents

      - [Modules](#Modules)
      - [Plugins](#Plugins)
      - [Scripts](#Scripts)
      - [Tools](#Tools)
      - [Installing MSF](#Installing\MSF)
  - [MSF Engagement Structure](#MSF\Engagement\Structure)


#### Modules

The Modules detailed above are split into separate categories in this folder. We will go into detail about these in the next sections. They are contained in the following folders:

Introduction to Metasploit

```shell
gdxqpardo@htb[/htb]$ ls /usr/share/metasploit-framework/modules

auxiliary  encoders  evasion  exploits  nops  payloads  post
```


#### Plugins

Plugins offer the pentester more flexibility when using the `msfconsole` since they can easily be manually or automatically loaded as needed to provide extra functionality and automation during our assessment.

```shell
gdxqpardo@htb[/htb]$ ls /usr/share/metasploit-framework/plugins/

aggregator.rb      ips_filter.rb  openvas.rb           sounds.rb
alias.rb           komand.rb      pcap_log.rb          sqlmap.rb
auto_add_route.rb  lab.rb         request.rb           thread.rb
beholder.rb        libnotify.rb   rssfeed.rb           token_adduser.rb
db_credcollect.rb  msfd.rb        sample.rb            token_hunter.rb
db_tracker.rb      msgrpc.rb      session_notifier.rb  wiki.rb
event_tester.rb    nessus.rb      session_tagger.rb    wmap.rb
ffautoregen.rb     nexpose.rb     socket_logger.rb
```


#### Scripts
Meterpreter functionality and other useful scripts
```shell
gdxqpardo@htb[/htb]$ ls /usr/share/metasploit-framework/scripts/

meterpreter  ps  resource  shell
```

#### Tools

Command-line utilities that can be called directly from the `msfconsole` menu.

```shell
gdxqpardo@htb[/htb]$ ls /usr/share/metasploit-framework/tools/

context  docs     hardware  modules   payloads
dev      exploit  memdump   password  recon
```

#### Installing MSF
```bash
gdxqpardo@htb[/htb]$ sudo apt update && sudo apt install metasploit-framework
```

The first step covered in this module is searching for a proper `exploit` for our target. Nevertheless, we need to have a detailed perspective on the `target` itself before attempting any exploitation. This involves the `enumeration` process, which precedes any type of exploitation

## MSF Engagement Structure

The MSF engagement structure can be divided into five main categories.

- Enumeration
- Preparation
- Exploitation
- Privilege Escalation
- Post-Exploitation
![[S04_SS03.png]]







