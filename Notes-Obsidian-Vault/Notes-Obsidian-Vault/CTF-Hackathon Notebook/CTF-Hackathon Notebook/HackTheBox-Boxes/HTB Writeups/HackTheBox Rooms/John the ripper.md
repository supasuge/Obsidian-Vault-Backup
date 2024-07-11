## Table of Contents

      - [How to Create Custom rules for single crack mode](#How\to\Create\Custom\rules\for\single\crack\mode)
      - [Using Custom Rules](#Using\Custom\Rules)
  - [Cracking Password Protected Zip Files](#Cracking\Password\Protected\Zip\Files)
- [Zip2john](#zip2john)
      - [Cracking a Password Protected RAR Archive](#Cracking\a\Password\Protected\RAR\Archive)
      - [](#)
      - [Rar2John](#Rar2John)
      - [Cracking SSH Key Passwords](#Cracking\SSH\Key\Passwords)
      - [](#)
      - [SSH2John](#SSH2John)

#### How to Create Custom rules for single crack mode

found in~ /etc/john/john.conf or /opt/john/john.conf

[List.Rules:THMRules] - Used to define the name of your rule, this is what you use to call your customer rule as a john argument

use regex style pattern match to define where in the word will be modified,
Az - Takes the word and appends it with the character you define
A0 - Takes the word and prepends it with characters you define
c - Capitalises the character positionally

Lastly, we then need to define what characters should be appended, prepended or otherwise included, we do this by adding character sets in square brackets `[ ]` in the order they should be used. These directly follow the modifier patterns inside of double quotes `" "`. Here are some common examples:

  

`[0-9]` - Will include numbers 0-9  

`[0]` - Will include only the number 0  

`[A-z]` - Will include both upper and lowercase  

`[A-Z]` - Will include only uppercase letters  

`[a-z]` - Will include only lowercase letters  

`[a]` - Will include only a  

`[!£$%@]` - Will include the symbols !£$%@  

  

Putting this all together, in order to generate a wordlist from the rules that would match the example password "Polopassword1!" (assuming the word polopassword was in our wordlist) we would create a rule entry that looks like this:

`[List.Rules:PoloPassword]`

`cAz"[0-9] [!£$%@]"`

  

In order to:

Capitalise the first  letter - `c`

Append to the end of the word - `Az`

A number in the range 0-9 - `[0-9]`

Followed by a symbol that is one of `[!£$%@]`

  

#### Using Custom Rules

We could then call this custom rule as a John argument using the  `--rule=PoloPassword` flag.  

As a full command: `john --wordlist=[path to wordlist] --rule=PoloPassword [path to file]`

  

As a note I find it helpful to talk out the patterns if you're writing a rule- as shown above, the same applies to writing RegEx patterns too.  

Jumbo John already comes with a large list of custom rules, which contain modifiers for use almost all cases. If you get stuck, try looking at those rules [around line 678] if your syntax isn't working properly

## Cracking Password Protected Zip Files

# Zip2john
zip2john converts the zip file in a hash format the john is able to understamd and hopefully crack. `zip2john [options] [zip file] > [output file]`
`[options]` - Allows you to pass specific checksum options to zip2john, this shouldn't often be necessary  

`[zip file]` - The path to the zip file you wish to get the hash of

`>` - This is the output director, we're using this to send the output from this file to the...  

`[output file]` - This is the file that will store the output from

#### Cracking a Password Protected RAR Archive

We can use a similar process to the one we used in the last task to obtain the password for rar archives. If you aren't familiar, rar archives are compressed files created by the Winrar archive manager. Just like zip files they compress a wide variety of folders and files.

####   

#### Rar2John

Almost identical to the zip2john tool that we just used, we're going to use the rar2john tool to convert the rar file into a hash format that John is able to understand. The basic syntax is as follows:  

`rar2john [rar file] > [output file]   `

`rar2john` - Invokes the rar2john tool  

`[rar file]` - The path to the rar file you wish to get the hash of

`>` - This is the output director, we're using this to send the output from this file to the...  

`[output file]` - This is the file that will store the output f

#### Cracking SSH Key Passwords  

Okay, okay I hear you, no more file archives! Fine! Let's explore one more use of John that comes up semi-frequently in CTF challenges. Using John to crack the SSH private key password of id_rsa files. Unless configured otherwise, you authenticate your SSH login using a password. However, you can configure key-based authentication, which lets you use your private key, id_rsa, as an authentication key to login to a remote machine over SSH. However, doing so will often require a password- here we will be using John to crack this password to allow authentication over SSH using the key.  

####   

#### SSH2John

Who could have guessed it, another conversion tool? Well, that's what working with John is all about. As the name suggests ssh2john converts the id_rsa private key that you use to login to the SSH session into hash format that john can work with. Jokes aside, it's another beautiful example of John's versatility. The syntax is about what you'd expect. Note that if you don't have ssh2john installed, you can use ssh2john.py, which is located in the /opt/john/ssh2john.py. If you're doing this, replace the `ssh2john` command with `python3 /opt/ssh2john.py` or on Kali, `python /usr/share/john/ssh2john.py`.

`ssh2john [id_rsa private key file] > [output file]   `

ssh2john - Invokes the ssh2john tool  

`[id_rsa private key file]` - The path to the id_rsa file you wish to get the hash of

`>` - This is the output director, we're using this to send the output from this file to the...  

`[output file]` - This is the file that will store the output from




