## Table of Contents

  - [Table of Contents](#Table\of\Contents)
      - [Why?](#Why?)

## Table of Contents

      - [Why?](#Why?)

#### Why?
- Target Specific Wordlists
- Depth of Search
- Customisable Outputs
- Built-in features
- Efficiency
- Integration
- Actively Maintained

```bash
cewl -h
CeWL 6.1 (Max Length) Robin Wood (robin@digi.ninja) (https://digi.ninja/)
Usage: cewl [OPTIONS] ... 

    OPTIONS:
	-h, --help: Show help.
	
	-k, --keep: Keep the downloaded file.
	
	-d ,--depth : Depth to spider to, default 2.
	
	-m, --min_word_length: Minimum word length, default 3.
	
	-x, --max_word_length: Maximum word length, default unset.
	
	-o, --offsite: Let the spider visit other sites.
	
	--exclude: A file containing a list of paths to exclude
	
	--allowed: A regex pattern that path must match to be followed
	
	-w, --write: Write the output to the file.
	
	-u, --ua : User agent to send.
	
	-n, --no-words: Don't output the wordlist.
	
	-g , --groups : Return groups of words as well
	
	--lowercase: Lowercase all parsed words
	
	--with-numbers: Accept words with numbers in as well as just letters
	
	--convert-umlauts: Convert common ISO-8859-1 (Latin-1) umlauts (ä-ae, ö-oe, ü-ue, ß-ss)
	
	-a, --meta: include meta data.
	
	--meta_file file: Output file for meta data.
	
	-e, --email: Include email addresses.
	
	--email_file : Output file for email addresses.
	
	--meta-temp-dir : The temporary directory used by exiftool when parsing files, default /tmp.
	
	-c, --count: Show the count for each word found.
	-v, --verbose: Verbose.
	
	--debug: Extra debug information.
	
[--snip--]
```



**Create a password list using CeWL:** Use the AntarctiCrafts homepage to generate a wordlist that could potentially hold the key to the portal.

Terminal

           `user@tryhackme$ cewl -d 2 -m 5 -w passwords.txt http://10.10.195.203 --with-numbers user@tryhackme$ cat passwords.txt`


