## Table of Contents

      - [Hashcat - Syntax](#Hashcat\-\Syntax)
  - [Hashid](#Hashid)
- [Hashcat Attack modes\[ -a \]](#hashcat\attack\modes\[\-a\\])
      - [Hashcat - Optimizations](#Hashcat\-\Optimizations)
      - [Hashcat - Syntax](#Hashcat\-\Syntax)
      - [Creating MD5 hashes](#Creating\MD5\hashes)
      - [Hashcat - Mask Attack](#Hashcat\-\Mask\Attack)
      - [Hashcat - Hybrid Attack using Wordlists](#Hashcat\-\Hybrid\Attack\using\Wordlists)
- [Creating Custom Wordlists](#creating\custom\wordlists)
      - [Crunch - Generate Word List](#Crunch\-\Generate\Word\List)
      - [Crunch - Create Word List using Pattern](#Crunch\-\Create\Word\List\using\Pattern)
- [CUPP](#cupp)
      - [Kwprocessor - Installation](#Kwprocessor\-\Installation)
      - [Princeprocessor - Installation](#Princeprocessor\-\Installation)
      - [Princeprocessor - Find the Number of Combinations](#Princeprocessor\-\Find\the\Number\of\Combinations)
      - [Princeprocessor - Forming Wordlist](#Princeprocessor\-\Forming\Wordlist)
      - [Princeprocessor - Password Length Limits](#Princeprocessor\-\Password\Length\Limits)
      - [Princeprocessor - Specifying Elements](#Princeprocessor\-\Specifying\Elements)
      - [CeWL - Syntax](#CeWL\-\Syntax)
  - [Hashcat-utils](#Hashcat-utils)
  - [Example Rule Creation](#Example\Rule\Creation)
      - [Rules](#Rules)
      - [Create a Rule File](#Create\a\Rule\File)
      - [Store the Password in a File](#Store\the\Password\in\a\File)
      - [Hashcat - Default Rules](#Hashcat\-\Default\Rules)
      - [Hashcat - Cracking Passwords Using Wordlists and Rules](#Hashcat\-\Cracking\Passwords\Using\Wordlists\and\Rules)
      - [Hashcat - Generate Random Rules](#Hashcat\-\Generate\Random\Rules)



#### Hashcat - Syntax

  Hashcat - Syntax

```shell
gdxqpardo@htb[/htb]$ hashcat -a 0 -m <hash type> <hash file> <wordlist>
```

Some hash functions can be keyed (i.e., an additional secret is used to create the hash). One such example is [HMAC](https://en.wikipedia.org/wiki/HMAC), which acts as a [checksum](https://en.wikipedia.org/wiki/Checksum) to verify if a particular message was tampered with during transmission.

For example, mainly four different algorithms can be used to protect passwords on Unix systems. These are `SHA-512`, `Blowfish`, `BCrypt`, and `Argon2`. These algorithms are available on Unix operating systems such as Linux, BSD, and Solaris.

The hash `$6$vb1tLY1qiY$M.1ZCqKtJBxBtZm1gRi8Bbkn39KU0YJW1cuMFzTRANcNKFKR4RmAQVk4rqQQCkaJT6wXqjUkFcA/qNxLyqW.U/` contains three fields delimited by `$`, where the first field is the `id`, i.e., `6`. This is used to identify the type of algorithm used for hashing. The following list contains some ids and their corresponding algorithms.

```shell
$1$  : MD5
$2a$ : Blowfish
$2y$ : Blowfish, with correct handling of 8 bit characters
$5$  : SHA256
$6$  : SHA512
```
## Hashid

[Hashid](https://github.com/psypanda/hashID) is a `Python` tool, which can be used to detect various kinds of hashes. At the time of writing, `hashid` can be used to identify over 200 unique hash types, and for others, it will make a best-effort guess, which will still require some additional work to narrow it down. The full list of supported hashes can be found [here](https://github.com/psypanda/hashID/blob/master/doc/HASHINFO.xlsx). It can be installed using `pip`.

```shell
gdxqpardo@htb[/htb]$ pip install hashid
```

Hashes can be supplied as command-line arguments or using a file.

```shell
gdxqpardo@htb[/htb]$ hashid '$apr1$71850310$gh9m4xcAn3MGxogwX/ztb.'

Analyzing '$apr1$71850310$gh9m4xcAn3MGxogwX/ztb.'
[+] MD5(APR) 
[+] Apache MD5
```

```shell
gdxqpardo@htb[/htb]$ hashid hashes.txt 
```

If known, `hashid` can also provide the corresponding `Hashcat` hash mode with the `-m` flag if it is able to determine the hash type.

```shell
gdxqpardo@htb[/htb]$ hashid '$DCC2$10240#tom#e4e938d12fe5974dc42a90120bd9c90f' -m
```
https://hashcat.net/wiki/doku.php?id=example_hashes
https://github.com/hashcat/hashcat
https://crackstation.net/crackstation-wordlist-password-cracking-dictionary.htm
# Hashcat Attack modes\[ -a \]

|**#**|**Mode**|
|---|---|
|0|Straight|
|1|Combination|
|3|Brute-force|
|6|Hybrid Wordlist + Mask|
|7|Hybrid Mask + Wordlist|

#### Hashcat - Optimizations

Hashcat has two main ways to optimize speed:

|Option|Description|
|---|---|
|Optimized Kernels|This is the `-O` flag, which according to the documentation, means `Enable optimized kernels (limits password length)`. The magical password length number is generally 32, with most wordlists won't even hit that number. This can take the estimated time from days to hours, so it is always recommended to run with `-O` first and then rerun after without the `-O` if your GPU is idle.|
|Workload|This is the `-w` flag, which, according to the documentation, means `Enable a specific workload profile`. The default number is `2`, but if you want to use your computer while Hashcat is running, set this to `1`. If you plan on the computer only running Hashcat, this can be set to `3`.|

We can see what `Hashcat` will produce given the same two files in the following example:

```shell-session
gdxqpardo@htb[/htb]$ hashcat -a 1 --stdout file1 file2
superhello
superpassword
worldhello
worldpassword
secrethello
secretpassword
```

#### Hashcat - Syntax

The syntax for the combination attack is:

  Hashcat - Syntax

```shell-session
gdxqpardo@htb[/htb]$ hashcat -a 1 -m <hash type> <hash file> <wordlist1> <wordlist2>
```

**Mask attack placeholders**

|**Placeholder**|**Meaning**|
|---|---|
|?l|lower-case ASCII letters (a-z)|
|?u|upper-case ASCII letters (A-Z)|
|?d|digits (0-9)|
|?h|0123456789abcdef|
|?H|0123456789ABCDEF|
|?s|special characters («space»!"#$%&'()\*+,-./:;<=>?@[]^_\`{|
|?a|?l?u?d?s|
|?b|0x00 - 0xff|


Consider the company Inlane Freight, which this time has passwords with the scheme "`ILFREIGHT<userid><year>`," where userid is 5 characters long. The mask "`ILFREIGHT?l?l?l?l?l20[0-1]?d`" can be used to crack passwords with the specified pattern, where "`?l`" is a letter and "`20[0-1]?d`" will include all years from 2000 to 2019.

Let's try creating a hash and cracking it using this mask.

#### Creating MD5 hashes

  Creating MD5 hashes

```shell-session
gdxqpardo@htb[/htb]$ echo -n 'ILFREIGHTabcxy2015' | md5sum | tr -d " -" > md5_mask_example_hash
```

In the below example, the attack mode is `3`, and the hash type for MD5 is `0`.

#### Hashcat - Mask Attack

  Hashcat - Mask Attack

```shell-session
gdxqpardo@htb[/htb]$ hashcat -a 3 -m 0 md5_mask_example_hash -1 01 'ILFREIGHT?l?l?l?l?l20?1?d'
```

#### Hashcat - Hybrid Attack using Wordlists

  Hashcat - Hybrid Attack using Wordlists

```shell
gdxqpardo@htb[/htb]$ hashcat -a 6 -m 0 hybrid_hash /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt '?d?s'
```
Attack mode "`7`" can be used to prepend characters to words using a given mask. The following example shows a mask using a custom char


# Creating Custom Wordlists
https://sourceforge.net/projects/crunch-wordlist/

The "`-t`" option is used to specify the pattern for generated passwords. The pattern can contain "`@`," representing lower case characters, "`,`" (comma) will insert upper case characters, "`%`" will insert numbers, and "`^`" will insert symbols.

#### Crunch - Generate Word List

  Crunch - Generate Word List

```shell
gdxqpardo@htb[/htb]$ crunch 4 8 -o wordlist
```

The command above creates a wordlist consisting of words with a length of 4 to 8 characters, using the default character set.

Let's assume that Inlane Freight user passwords are of the form "`ILFREIGHTYYYYXXXX`," where "`XXXX`" is the employee ID containing letters, and "`YYYY`" is the year. We can use `crunch` to create a list of such passwords.

#### Crunch - Create Word List using Pattern

  Crunch - Create Word List using Pattern

```shell
gdxqpardo@htb[/htb]$ crunch 17 17 -t ILFREIGHT201%@@@@ -o wordlist
```
The pattern "`ILFREIGHT201%@@@@`" will create words with the years 2010-2019 followed by four letters. The length here is 17, which is constant for all words.

If we know a user's birthdate is 10/03/1998 (through social media, etc.), we can include this in their password, followed by a string of letters. Crunch can be used to create a wordlist of such words. The "`-d`" option is used to specify the amount of repetition.
# CUPP
`CUPP` stands for `Common User Password Profiler`, and is used to create highly targeted and customized wordlists based on information gained from social engineering and OSINT. People tend to use personal information while creating passwords, such as phone numbers, pet names, birth dates, etc. CUPP takes in this information and creates passwords from them. These wordlists are mostly used to gain access to social media accounts. CUPP is installed by default on Parrot OS, and the repo can be found [here](https://github.com/Mebus/cupp). The "`-i`" option is used to run in interactive mode, prompting `CUPP` to ask us for information on the target.

`Kwprocessor` is a tool that creates wordlists with keyboard walks. Another common password generation technique is to follow patterns on the keyboard. These passwords are called keyboard walks, as they look like a walk along the keys. For example, the string "`qwertyasdfg`" is created by using the first five characters from the keyboard's first two rows. This seems complex to the normal eye but can be easily predicted. `Kwprocessor` uses various algorithms to guess patterns such as these.

The tool can be found [here](https://github.com/hashcat/kwprocessor) and has to be installed manually.

#### Kwprocessor - Installation

  Kwprocessor - Installation

```shell
gdxqpardo@htb[/htb]$ git clone https://github.com/hashcat/kwprocessor
gdxqpardo@htb[/htb]$ cd kwprocessor
gdxqpardo@htb[/htb]$ make
```

The `PRINCE` algorithm considers various permutation and combinations while creating each word. The binary can be download from the [releases](https://github.com/hashcat/princeprocessor/releases) page.

#### Princeprocessor - Installation

  Princeprocessor - Installation

```shell
gdxqpardo@htb[/htb]$ wget https://github.com/hashcat/princeprocessor/releases/download/v0.22/princeprocessor-0.22.7z
gdxqpardo@htb[/htb]$ 7z x princeprocessor-0.22.7z
gdxqpardo@htb[/htb]$ cd princeprocessor-0.22
gdxqpardo@htb[/htb]$ ./pp64.bin -h
```

#### Princeprocessor - Find the Number of Combinations

  Princeprocessor - Find the Number of Combinations

```shell-session
gdxqpardo@htb[/htb]$ ./pp64.bin --keyspace < words

232
```

According to princeprocessor, 232 unique words can be formed from our wordlist above.

#### Princeprocessor - Forming Wordlist

  Princeprocessor - Forming Wordlist

```shell-session
gdxqpardo@htb[/htb]$ ./pp64.bin -o wordlist.txt < words
```

The command above writes the output words to a file named `wordlist.txt`. By default, princeprocessor only outputs words up to 16 in length. This can be controlled using the "`--pw-min`" and "`--pw-max`" arguments.

#### Princeprocessor - Password Length Limits

  Princeprocessor - Password Length Limits

```shell-session
gdxqpardo@htb[/htb]$ ./pp64.bin --pw-min=10 --pw-max=25 -o wordlist.txt < words
```

The command above will output words between 10 and 25 in length. The number of elements per word can be controlled using "`--elem-cnt-min`" and "`--elem-cnt-max`". These values ensure that number of elements in an output word is above or below the given value.

#### Princeprocessor - Specifying Elements

  Princeprocessor - Specifying Elements

```shell-session
gdxqpardo@htb[/htb]$ ./pp64.bin --elem-cnt-min=3 -o wordlist.txt < words
```

The command above will output words with three elements or more, i.e.," dogdogdog`."


#### CeWL - Syntax

  CeWL - Syntax

```shell-session
gdxqpardo@htb[/htb]$ cewl -d <depth to spider> -m <minimum word length> -w <output wordlist> <url of website>
```

CeWL can spider multiple pages present on a given website. The length of the outputted words can be altered using the "`-m`" parameter, depending on the password requirements (i.e., some websites have a minimum password length).

By default, hashcat stores all cracked passwords in the `hashcat.potfile` file; the format is `hash:password`. The main purpose of this file is to remove previously cracked hashes from the work log and display cracked passwords with the `--show` command. However, it can be used to create new wordlists of previously cracked passwords, and when combined with rule files, it can prove quite effective at cracking themed passwords.

  CeWL - Example

```shell-session
gdxqpardo@htb[/htb]$ cut -d: -f 2- ~/hashcat.potfile
```

---

## Hashcat-utils

The Hashcat-utils [repo](https://github.com/hashcat/hashcat-utils) contains many utilities that can be useful for more advanced password cracking. The tool [maskprocessor](https://github.com/hashcat/maskprocessor), for example, can be used to create wordlists using a given mask. Detailed usage for this tool can be found [here](https://hashcat.net/wiki/doku.php?id=maskprocessor).

For example, `maskprocessor` can be used to append all special characters to the end of a word:

  CeWL - Example

```shell-session
gdxqpardo@htb[/htb]$ /mp64.bin Welcome?s
```

|**Function**|**Description**|**Input**|**Output**|
|---|---|---|---|
|l|Convert all letters to lowercase|InlaneFreight2020|inlanefreight2020|
|u|Convert all letters to uppercase|InlaneFreight2020|INLANEFREIGHT2020|
|c / C|capitalize / lowercase first letter and invert the rest|inlaneFreight2020 / Inlanefreight2020|Inlanefreight2020 / iNLANEFREIGHT2020|
|t / TN|Toggle case : whole word / at position N|InlaneFreight2020|iNLANEfREIGHT2020|
|d / q / zN / ZN|Duplicate word / all characters / first character / last character|InlaneFreight2020|InlaneFreight2020InlaneFreight2020 / IInnllaanneeFFrreeiigghhtt22002200 / IInlaneFreight2020 / InlaneFreight20200|
|{ / }|Rotate word left / right|InlaneFreight2020|nlaneFreight2020I / 0InlaneFreight202|
|^X / $X|Prepend / Append character X|InlaneFreight2020 (^! / $! )|!InlaneFreight2020 / InlaneFreight2020!|
|r|Reverse|InlaneFreight2020|0202thgierFenalnI|

---

A complete list of functions can be found [here](https://hashcat.net/wiki/doku.php?id=rule_based_attack#implemented_compatible_functions). Sometimes, the input wordlists contain words that don't match our target specifications. For example, a company's password policy might not allow users to set passwords less than 7 characters in length. In such cases, rejection rules can be used to prevent the processing of such words.

Words of length less than N can be rejected with `>N`, while words greater than N can be rejected with `<N`. A list of rejection rules can be found [here](https://hashcat.net/wiki/doku.php?id=rule_based_attack#rules_used_to_reject_plains).

_Note: Reject rules only work either with `hashcat-legacy`, or when using `-j` or `-k` with `Hashcat`. They will not work as regular rules (in a rule file) with `Hashcat`._

---

## Example Rule Creation

Let's look at how we can create a rule based on common passwords. Usual user behavior suggests that they tend to replace letters with similar numbers, like "`o`" can be replaced with "`0`" or "`i`" can be replaced with "`1`". This is commonly known as `L33tspeak` and is very efficient. Corporate passwords are often prepended or appended by a year. Let's create a rule to generate such words.

*Note: Reject rules only work either with `hashcat-legacy`, or when using `-j` or `-k` with `Hashcat`. They will not work as regular rules (in a rule file) with `Hashcat`. *

#### Rules

  Rules

```shell-session
c so0 si1 se3 ss5 sa@ $2 $0 $1 $9
```

The first letter word is capitalized with the `c` function. Then rule uses the substitute function `s` to replace `o` with `0`, `i` with `1`, `e` with `3` and a with `@`. At the end, the year `2019` is appended to it. Copy the rule to a file so that we can debug it.

#### Create a Rule File

  Create a Rule File

```shell-session
gdxqpardo@htb[/htb]$ echo 'c so0 si1 se3 ss5 sa@ $2 $0 $1 $9' > rule.txt
```

#### Store the Password in a File

  Store the Password in a File

```shell-session
gdxqpardo@htb[/htb]$ echo 'password_ilfreight' > test.txt
```

---

Rules can be debugged using the "`-r`" flag to specify the rule, followed by the wordlist.

#### Hashcat - Default Rules

  Hashcat - Default Rules

```shell
gdxqpardo@htb[/htb]$ ls -l /usr/share/hashcat/rules/

total 2576
-rw-r--r-- 1 root root    933 Jun 19 06:20 best64.rule
-rw-r--r-- 1 root root    633 Jun 19 06:20 combinator.rule
-rw-r--r-- 1 root root 200188 Jun 19 06:20 d3ad0ne.rule
-rw-r--r-- 1 root root 788063 Jun 19 06:20 dive.rule
-rw-r--r-- 1 root root 483425 Jun 19 06:20 generated2.rule
-rw-r--r-- 1 root root  78068 Jun 19 06:20 generated.rule
drwxr-xr-x 1 root root   2804 Jul  9 21:01 hybrid
-rw-r--r-- 1 root root 309439 Jun 19 06:20 Incisive-leetspeak.rule
-rw-r--r-- 1 root root  35280 Jun 19 06:20 InsidePro-HashManager.rule
-rw-r--r-- 1 root root  19478 Jun 19 06:20 InsidePro-PasswordsPro.rule
-rw-r--r-- 1 root root    298 Jun 19 06:20 leetspeak.rule
-rw-r--r-- 1 root root   1280 Jun 19 06:20 oscommerce.rule
-rw-r--r-- 1 root root 301161 Jun 19 06:20 rockyou-30000.rule
-rw-r--r-- 1 root root   1563 Jun 19 06:20 specific.rule
-rw-r--r-- 1 root root  64068 Jun 19 06:20 T0XlC-insert_00-99_1950-2050_toprules_0_F.rule
-rw-r--r-- 1 root root   2027 Jun 19 06:20 T0XlC-insert_space_and_special_0_F.rule
-rw-r--r-- 1 root root  34437 Jun 19 06:20 T0XlC-insert_top_100_passwords_1_G.rule
-rw-r--r-- 1 root root  34813 Jun 19 06:20 T0XlC.rule
-rw-r--r-- 1 root root 104203 Jun 19 06:20 T0XlCv1.rule
-rw-r--r-- 1 root root     45 Jun 19 06:20 toggles1.rule
-rw-r--r-- 1 root root    570 Jun 19 06:20 toggles2.rule
-rw-r--r-- 1 root root   3755 Jun 19 06:20 toggles3.rule
-rw-r--r-- 1 root root  16040 Jun 19 06:20 toggles4.rule
-rw-r--r-- 1 root root  49073 Jun 19 06:20 toggles5.rule
-rw-r--r-- 1 root root  55346 Jun 19 06:20 unix-ninja-leetspeak.rule
```
There are a variety of publicly available rules as well, such as the [nsa-rules](https://github.com/NSAKEY/nsa-rules), [Hob0Rules](https://github.com/praetorian-code/Hob0Rules), and the [corporate.rule](https://github.com/sparcflow/HackLikeALegend/blob/master/old/chap3/corporate.rule) which is featured in the book [How to Hack Like a Legend](https://www.sparcflow.com/new-release-hack-like-legend/). These are curated rulesets generally targeted at common corporate Windows password policies or based on statistics and probably industry password patterns.

#### Hashcat - Cracking Passwords Using Wordlists and Rules

  Hashcat - Cracking Passwords Using Wordlists and Rules

```shell
gdxqpardo@htb[/htb]$ hashcat -a 0 -m 100 hash /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt -r rule.txt
```
#### Hashcat - Generate Random Rules

  Hashcat - Generate Random Rules

```shell-session
gdxqpardo@htb[/htb]$ hashcat -a 0 -m 100 -g 1000 hash /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt
```