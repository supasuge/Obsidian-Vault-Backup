## Programming Guidelines

The following guidelines for clean programming will enable us to distinguish clear code from bad code and convert bad code into good code. This becomes especially important when we work with different code or when we need to adapt or modify it. The following five guidelines alone will help us keep our code simple, structured, and efficient.

|**Guideline**|**Description**|
|---|---|
|`Unique names`|Name variables, functions, and classes with consistent, meaningful, pronounceable, and distinguishable names. These types should be different and clearly identifiable from each other. You can use upper and lower case letters for this.|
|`Avoid error-prone constructs`|Design control statements in the easiest way. Avoid heavily nested control statements as they are difficult to test and understand.|
|`One task per function`|Each function should only ever perform one single task. This will later help to understand where the errors occur and to be able to fix them quickly.|
|`Duplicating lines of code are prohibited`|We must avoid duplicating written code in any case. Otherwise, it will become too big and confusing too fast. Besides, we may also reproduce bugs that will make our code almost impossible to execute until we fix all of them.|
|`Small number of method arguments`|The fewer arguments have to be used for a function, the easier the function is to understand. Furthermore, the error rate is much lower, and fixing the error is also much more straightforward and, above all, much more time-saving.|

## PEP8

There are guidelines developed especially for Python, known as the `Python Enhancement Proposal` (`PEP8`). [PEP8](https://www.python.org/dev/peps/pep-0008/) is a document that contains `guidelines` and `best practices` for writing Python code and was written in 2001 by Guido van Rossum, Barry Warsaw, and Nick Coghlan. `PEP8` 's primary focus is to improve the `readability` and `consistency` of Python code. When we have more experience in writing Python code, we may start working with others. Writing readable code is crucial because other people who are not familiar with our `coding style` need to read and understand our code. If we have guidelines that we follow and recognize, others will also find our code easier to read.

#### PEP8 Guidelines

|**Rule**|**Description**|
|---|---|
|`Indentation`|4 spaces, no tabs!|
|`Max. Line Length for Code`|79 characters / line|
|`Max. Line Length for Comments/DocStrings`|72 characters / line|
|`Encoding`|UTF-8 for Python 3|
|`Quotes`|The use of single and double quotes is possible, but we should decide on one of them.|
|`...`|...|

## Modules
`dnspython` - https://dnspython.readthedocs.io/en/latest/

After we have installed the module, we can import it. This module offers us the classes `Query`, `Resolver`, and `Zone`. The `dns.zone` module contains a "`from_xfr`" class. We can find this out by using the `help()` function in Python.


#### Dns.Zone.From_XFR()

Python Modules

```shell-session
In [2]: help(dz.from_xfr) 
```

#### From_XFR() - Documentation

```shell
from_xfr(xfr, zone_factory=<class 'dns.zone.Zone'>, relativize=True, check_origin=True)
    Convert the output of a zone transfer generator into a zone object.

    @param xfr: The xfr generator
    @type xfr: generator of dns.message.Message objects
    @param relativize: should names be relativized?  The default is True.
    It is essential that the relativize setting matches the one specified
    to dns.query.xfr().
    @type relativize: bool
    @param check_origin: should sanity checks of the origin node be done?
    The default is True.
    @type check_origin: bool
    @raises dns.zone.NoSOA: No SOA RR was found at the zone origin
    @raises dns.zone.NoNS: No NS RRset was found at the zone origin
    @rtype: dns.zone.Zone object
```

We can also find the documentation for this on the [documentation page](https://dnspython.readthedocs.io/en/latest/zone-make.html) that describes the required parameters for the `from_xfr()` function. From the parameter "`xfr`," we will also need the `dns.query` class. So we should also note this class for later use.

#### Notes

Code: python

```python
# Notes
import dns.zone as dz
import dns.query as dq

# axfr = dz.from_xfr(dq.xfr())
```

We need to take a closer look at the `dns.query.xfr()` function to determine which parameters are required.

#### Dns.Query.XFR()

Python Modules

```shell-session
In [3]: help(dq.xfr) 
```

#### XFR() - Documentation

Python Modules

```shell-session
Help on function xfr in module dns.query:

xfr(where, zone, rdtype=252, rdclass=1, timeout=None, port=53, keyring=None, keyname=None, relativize=True, af=None, lifetime=None, source=None, source_port=0, serial=0, use_udp=False, keyalgorithm=<DNS name HMAC-MD5.SIG-ALG.REG.INT.>)
    Return a generator for the responses to a zone transfer.

    *where*.  If the inference attempt fails, AF_INET is used.  This
    parameter is historical; you need never set it.
<SNIP>
```

Here we can see that only two variables have no value and therefore need specific parameters to perform this function.

- `where`
- `zone`

We can also find out the required parameters by executing the function. Because if we do not output any parameters, an error will occur with the description that says which parameters are needed.

#### Required Parameters

Python Modules

```shell-session
In [4]: dq.xfr()

---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-14-672b143b8cfc> in <module>
----> 1 dq.xfr()

TypeError: xfr() missing 2 required positional arguments: 'where' and 'zone'
```

In this case, the variable "`where`" stands for the DNS server and the variable "`zone`" for the domain. We note this as well.

#### Notes

Code: python

```python
# Notes
import dns.zone as dz
import dns.query as dq

# axfr = dz.from_xfr(dq.xfr(nameserver, domain))
```


## DNS Requests

We have to find out how to resolve our requests by using specific DNS servers. The easiest way with the necessary class would be "`dns.resolver`". In the [documentation](https://dnspython.readthedocs.io/en/latest/resolver-class.html), we can find the class "`Resolver`" which allows us to specify the DNS servers we want to send our requests to. Of course, we can also automatically find these DNS servers, so we do not have to change them manually. Nevertheless, we have to be careful because companies often use DNS servers from third party providers for which we usually do not have the permissions to test them. Therefore, we recommend that we specify them manually and preferably in the command line rather than in our code. To make this possible, we import another module called "`argparse`". Accordingly, we also add this information to our notes.

#### Notes

Code: python

```python
# Notes
import dns.zone as dz
import dns.query as dq
import dns.resolver as dr
import argparse

# axfr = dz.from_xfr(dq.xfr(nameserver, domain))

# NS = dr.Resolver()
# NS.nameservers = ['ns1', 'ns2']
```

---

We now have to find the "NS" records for this domain, and instead of using the `dig` tool, we do it with our Python modules. In this example, we still use the domain called:

- `inlanefreight.com`

The corresponding NS servers we found by using the following code:

#### NS Records - DNS.Resolver

Python Modules

```shell-session
gdxqpardo@htb[/htb]$ python3

>>> import dns.resolver
>>> 
>>> nameservers = dns.resolver.query('inlanefreight.com', 'NS')
>>> for ns in nameservers:
...    	print('NS:', ns)
...
NS: ns1.inlanefreight.com.
NS: ns2.inlanefreight.com.
```

In summary, we have the following information now:

Code: python

```python
Domain = 'inlanefreight.com'
DNS Servers = ['ns1.inlanefreight.com', 'ns2.inlanefreight.com']
```

Now we can summarize all our information and write the first lines of our code.

#### DNS-AXFR.py

Code: python

```python
#!/usr/bin/env python3

# Dependencies:
# python3-dnspython

# Used Modules:
import dns.zone as dz
import dns.query as dq
import dns.resolver as dr
import argparse

# Initialize Resolver-Class from dns.resolver as "NS"
NS = dr.Resolver()

# Target domain
Domain = 'inlanefreight.com'

# Set the nameservers that will be used
NS.nameservers = ['ns1.inlanefreight.com', 'ns2.inlanefreight.com']

# List of found subdomains
Subdomains = []
```

## AXRF Function
It is always an advantage if we separate individual processes from each other and build a single function out of it. This makes it easier to debug later when errors occur, and it makes the main function more independent. It may seem irrelevant at first with such a small tool, but if we start to extend the code, and eventually, our tool grows to a size of more than 1000 lines of code, we will see this advantage. We can divide the process into the following sections:

1. We now want to create a function that tries to perform a zone transfer using the given domain and DNS servers.
    
2. If the zone transfer was successful, we want the found subdomains to be displayed directly and stored in our list.
    
3. In case an error occurs, we want to be informed about the error.
    

---

## Functions

For the `functions`, we should try to use as few passing arguments as possible. Therefore there should not be more than three arguments, because otherwise there can be a high error-proneness. So in the next example, we use the two arguments `domain` and `nameserver`, which we need for the `zone transfer`.

Again, we should determine how we define the `functions` and keep this standard. In this case, we define the functions by capitalizing all letters of them. Many people use this capitalization also for classes. Then we should add comments to the function to divide each step in the function into parts.

#### DNS-AXFR.py - Functions

Code: python

```python
<SNIP>
# List of found subdomains
Subdomains = []

# Define the AXFR Function
def AXFR(domain, nameserver):

        # Try zone transfer for given domain and namerserver
		# Perform the zone transfer
        # If zone transfer was successful
        # Add found subdomains to global 'Subdomain' list
        # If zone transfer fails

# Main
if __name__=="__main__":

        # For each nameserver
        # Try AXFR
        # Print the results
        # Print each subdomain
```

---

## Try-Except

If a statement or an expression is written correctly in terms of its syntax, errors may occur during execution. Errors that occur during execution are called `exceptions` and are not necessarily serious. A `try` statement can contain more than one `except` block to define different actions for different exceptions. At most, one `except` block is executed. A block can only handle the exceptions that occurred in the corresponding `try` block, but not those that occur in another except the block of the same `try` statement.

#### DNS-AXFR.py - Try-Except

Code: python

```python
<SNIP>
# List of found subdomains
Subdomains = []

# Define the AXFR Function
def AXFR(domain, nameserver):

        # Try zone transfer for given domain and namerserver
        try:
				# Perform the zone transfer
                axfr = dz.from_xfr(dq.xfr(nameserver, domain))
				
                # If zone transfer was successful
                # Add found subdomains to global 'Subdomain' list
				
        # If zone transfer fails
        except Exception as error:
                print(error)
                pass
```

---

## If-Else

With `if-else` statements, it depends on how many arguments or values we want to check simultaneously. In the best case, there should be only one value or argument to check at a time. However, if we need to check more than one argument and the line can be very long, it is recommended to write every argument in the brackets in a new line.

#### If-Else - Few Arguments

Code: python

```python
# Few arguments
if (arg1 and arg2):
	# Perform specified actions
else:
	pass
```

#### If-Else - Many Arguments

Code: python

```python
# Many arguments
if (arg1 
	and arg2
	and arg3
	and arg4):
	
	# Perform specified actions
else:
	pass
```

In our `DNS.py` script, we use only one tested value, and therefore we don't need multiple lines.

#### DNS-AXFR.py - If-Else

Code: python

```python
<SNIP>
# List of found subdomains
Subdomains = []

# Define the AXFR Function
def AXFR(domain, nameserver):

        # Try zone transfer for given domain and namerserver
        try:
				# Perform the zone transfer
                axfr = dz.from_xfr(dq.xfr(nameserver, domain))

                # If zone transfer was successful
                if axfr:
                        print('[*] Successful Zone Transfer from {}'.format(nameserver))

                        # Add found subdomains to global 'Subdomain' list

        # If zone transfer fails
        except Exception as error:
                print(error)
                pass
```

---

## For-Loop

The `For` loops should always be kept as simple as possible, as they can cause errors with the number of passes, which we then have to debug manually for each entry to understand what went wrong in the loop. Therefore, we will now append each "`record`" to our predefined "`Subdomains`" list to store the subdomains we found.

#### DNS-AXFR.py

Code: python

```python
<SNIP>
# List of found subdomains
Subdomains = []

# Define the AXFR Function
def AXFR(domain, nameserver):

        # Try zone transfer for given domain and namerserver
        try:
				# Perform the zone transfer
                axfr = dz.from_xfr(dq.xfr(nameserver, domain))

                # If zone transfer was successful
                if axfr:
                        print('[*] Successful Zone Transfer from {}'.format(nameserver))

                        # Add found subdomains to global 'Subdomain' list
                        for record in axfr:
                                Subdomains.append('{}.{}'.format(record.to_text(), domain))

        # If zone transfer fails
        except Exception as error:
                print(error)
                pass
```

Now we have our function, which only needs the domain and the respective name server as arguments. Next, we need the `Main function`, which passes the arguments to the `AXFR function`, executes it, and shows us the results.

## Main Function
The main function is the part of the code where the main program is running. It is essential to keep it as clear as possible, as we will most likely add more functions to our tool later. In this case, we want our tool to try a zone transfer on every DNS server we have specified. We know that the subdomains found will be added to the global subdomain list in the `AXFR()` function. So if this list is not empty, we want to see all subdomains.

---

## Main

If we start a Python program by calling a py file, and there we call `"__name__"`, the system assigns it to the system variable the identifier "`__main__`." However, if we import this file into another module, it gets an identifier corresponding to the file name. This construction uses a Python file as a stand-alone program, makes single elements of this file importable, and implements complex module tests. As we did with the `AXFR` function, we proceed accordingly and define the rough steps that need to be taken as comments.

#### DNS-AXFR.py

Code: python

```python
<SNIP>
# Main
if __name__=="__main__":

        # For each nameserver
        # Try AXFR
        # Print the results
        # Print each subdomain
```

---

## For-Loop - Try AXFR

Since our `AXFR` function requires the two parameters, "domain" and "nameserver," we create a `For-loop` that executes the `AXFR` function for each nameserver specified. We use another `For-loop` combined with the `If-Else` statement in the area that will output the found subdomains. If the subdomain list, in which the `AXFR` function stores the found subdomains, is not empty, then every subdomain entry from the subdomain list should be printed.

#### DNS-AXFR.py

Code: python

```python
<SNIP>
# Main
if __name__=="__main__":

        # For each nameserver
        for nameserver in NS.nameservers:

                #Try AXFR
                AXFR(Domain, nameserver)

        # Print the results
        if Subdomains is not None:
                print('-------- Found Subdomains:')

                # Print each subdomain
                for subdomain in Subdomains:
                        print('{}'.format(subdomain))

        else:
                print('No subdomains found.')
                exit()
```

---

## DNS-AXFR.py - Complete Code

Code: python

```python
#!/usr/bin/env python3

# Dependencies:
# python3-dnspython

# Used Modules:
import dns.zone as dz
import dns.query as dq
import dns.resolver as dr
import argparse

# Initialize Resolver-Class from dns.resolver as "NS"
NS = dr.Resolver()

# Target domain
Domain = 'inlanefreight.com'

# Set the nameservers that will be used
NS.nameservers = ['ns1.inlanefreight.com', 'ns2.inlanefreight.com']

# List of found subdomains
Subdomains = []

# Define the AXFR Function
def AXFR(domain, nameserver):

        # Try zone transfer for given domain and namerserver
        try:
				# Perform the zone transfer
                axfr = dz.from_xfr(dq.xfr(nameserver, domain))

                # If zone transfer was successful
                if axfr:
                        print('[*] Successful Zone Transfer from {}'.format(nameserver))

                        # Add found subdomains to global 'Subdomain' list
                        for record in axfr:
                                Subdomains.append('{}.{}'.format(record.to_text(), domain))

        # If zone transfer fails
        except Exception as error:
                print(error)
                pass

# Main
if __name__=="__main__":

        # For each nameserver
        for nameserver in NS.nameservers:

                #Try AXFR
                AXFR(Domain, nameserver)

        # Print the results
        if Subdomains is not None:
                print('-------- Found Subdomains:')

                # Print each subdomain
                for subdomain in Subdomains:
                        print('{}'.format(subdomain))

        else:
                print('No subdomains found.')
                exit()                
```

## Results

#### Make the Script Executable & Execute

Main Function

```shell-session
gdxqpardo@htb[/htb]$ chmod +x dns-axfr.py
gdxqpardo@htb[/htb]$ ./dns-axfr.py

[*] Successful Zone Transfer from ns1.inlanefreight.com
-------- Found Subdomains:
@.inlanefreight.com
admin.inlanefreight.com
app.inlanefreight.com
blog.inlanefreight.com
<SNIP>
```


## Argparse version
After determining that our script works properly, we can extend our code step by step and add more features. With the tool "dig," we have already seen that we can already define specific arguments in the terminal and pass them to the program. To avoid changing the code frequently, we can add the same function to our script and use it again when we need to. To include this function, we can use and import the standard module `argparse`.

Next, we define the arguments with the desired names in the Main function that our script should take from the terminal. It is highly recommended to test our script immediately if we have added anything new. Because if we add dozens of code lines with different dependencies and functions that affect others, it can significantly increase debugging time.

#### DNS-AXFR.py

Code: python

```python
<SNIP>
# Main
if __name__ == "__main__":

    # ArgParser - Define usage
    parser = argparse.ArgumentParser(prog="dns-axfr.py", epilog="DNS Zonetransfer Script", usage="dns-axfr.py [options] -d <DOMAIN>", prefix_chars='-', add_help=True)

<SNIP>
```

The passed arguments to the class `argparse` for the function "`ArgumentParser`" are "`prog`", "`usage`", "`prefix_chars`" and "`add_help`". The argument "`prog`" stands for the name of the script, which is then displayed in the help function ("`add_help`") with the usage example ("`usage`") if an argument is missing. The arguments are prefixed with the "`prefix_chars`" and are included in the script.

After initializing the parser, we can define the corresponding parameters, which we will define with our script's respective options. For this, we use the `add_argument()` method of the `ArgumentParser`. This method provides us with some parameters that we can use to define the option.

|**Parameter**|**Description**|
|---|---|
|[name or flag](https://docs.python.org/3/library/argparse.html#name-or-flags)|Either a name or a list of option strings.|
|[action](https://docs.python.org/3/library/argparse.html#action)|The basic type of action to be taken when this argument is encountered at the command line.|
|[nargs](https://docs.python.org/3/library/argparse.html#nargs)|The number of command-line arguments that should be consumed.|
|[const](https://docs.python.org/3/library/argparse.html#const)|A constant value required by some `action` and `nargs` selections.|
|[default](https://docs.python.org/3/library/argparse.html#default)|The value produced if the argument is absent from the command line.|
|[type](https://docs.python.org/3/library/argparse.html#type)|The type to which the command-line argument should be converted.|
|[choices](https://docs.python.org/3/library/argparse.html#choices)|A container of the allowable values for the argument.|
|[required](https://docs.python.org/3/library/argparse.html#required)|Whether or not the command-line option may be omitted (optionals only).|
|[help](https://docs.python.org/3/library/argparse.html#help)|A brief description of what the argument does.|
|[metavar](https://docs.python.org/3/library/argparse.html#metavar)|A name for the argument in usage messages.|
|[dest](https://docs.python.org/3/library/argparse.html#dest)|The name of the attribute to be added to the object returned by `parse_args()`.|

Source : [https://docs.python.org/3/library/argparse.html#the-add-argument-method](https://docs.python.org/3/library/argparse.html#the-add-argument-method)

Next, we define the target `domain` parameters and the `nameservers` on which we want to test the zone transfer. Since we can create another function later, which finds out the `nameservers` for the corresponding domain by itself, we define only the `domain` specification as `required`. Additionally, we add the script `version` to track later which version of the script we are using or editing. Finally, we assign the given arguments in the variable `args`.

#### DNS-AXFR.py

Code: python

```python
<SNIP>
# Main
if __name__ == "__main__":

    # ArgParser - Define usage
    parser = argparse.ArgumentParser(prog="dns-axfr.py", epilog="DNS Zonetransfer Script", usage="dns-axfr.py [options] -d <DOMAIN>", prefix_chars='-', add_help=True)
	
	# Positional Arguments
    parser.add_argument('-d', action='store', metavar='Domain', type=str, help='Target Domain.\tExample: inlanefreight.htb', required=True)
    parser.add_argument('-n', action='store', metavar='Nameserver', type=str, help='Nameservers separated by a comma.\tExample: ns1.inlanefreight.htb,ns2.inlanefreight.htb')
    parser.add_argument('-v', action='version', version='DNS-AXFR - v1.0', help='Prints the version of DNS-AXFR.py')

    # Assign given arguments
    args = parser.parse_args()

<SNIP>
```

Since we have replaced and extended the static definition of variables using `argparse`, we can remove the variable `domain` and `NS.nameservers` from the beginning of the script and adjust them in the `main`. To do this, we take the parameter `d` from the stored arguments in `args` ( = `args.d`), which stands for the variable `Domain`, and assign the passed argument to it.

The `NS.nameservers` variable can have several arguments, which we will separate with a comma (`,`). Therefore we create a `list`, which contains the arguments from `args.n` (nameservers) and separate them using the comma (`,`), if available.

#### DNS-AXFR.py

Code: python

```python
<SNIP>
# Main
if __name__ == "__main__":

    # ArgParser - Define usage
    parser = argparse.ArgumentParser(prog="dns-axfr.py", epilog="DNS Zonetransfer Script", usage="dns-axfr.py [options] -d <DOMAIN>", prefix_chars='-', add_help=True)
	
	# Positional Arguments
    parser.add_argument('-d', action='store', metavar='Domain', type=str, help='Target Domain.\tExample: inlanefreight.htb', required=True)
    parser.add_argument('-n', action='store', metavar='Nameserver', type=str, help='Nameservers separated by a comma.\tExample: ns1.inlanefreight.htb,ns2.inlanefreight.htb')
    parser.add_argument('-v', action='version', version='DNS-AXFR - v1.0', help='Prints the version of DNS-AXFR.py')

    # Assign given arguments
    args = parser.parse_args()

    # Variables
    Domain = args.d
    NS.nameservers = list(args.n.split(","))

    # Check if URL is given
    if not args.d:
        print('[!] You must specify target Domain.\n')
        print(parser.print_help())
        exit()

    if not args.n:
        print('[!] You must specify target nameservers.\n')
        print(parser.print_help())
        exit()

<SNIP>
```

The complete code would look like this.

#### DNS-AXFR.py - Complete Code

Code: python

```python
#!/usr/bin/env python3

# Dependencies:
# python3-dnspython

# Used Modules:
import dns.zone as dz
import dns.query as dq
import dns.resolver as dr
import argparse

# Initialize Resolver-Class from dns.resolver as "NS"
NS = dr.Resolver()

# List of found subdomains
Subdomains = []

# Define the AXFR Function
def AXFR(domain, nameserver):

    # Try zone transfer for given domain and namerserver
    try:
        # Perform the zone transfer
        axfr = dz.from_xfr(dq.xfr(nameserver, domain))

        # If zone transfer was successful
        if axfr:
            print('[*] Successful Zone Transfer from {}'.format(nameserver))

            # Add found subdomains to global 'Subdomain' list
            for record in axfr:
                Subdomains.append('{}.{}'.format(record.to_text(), domain))

    # If zone transfer fails
    except Exception as error:
        print(error)
        pass

# Main
if __name__ == "__main__":

    # ArgParser - Define usage
    parser = argparse.ArgumentParser(prog="dns-axfr.py", epilog="DNS Zonetransfer Script", usage="dns-axfr.py [options] -d <DOMAIN>", prefix_chars='-', add_help=True)

    # Positional Arguments
    parser.add_argument('-d', action='store', metavar='Domain', type=str, help='Target Domain.\tExample: inlanefreight.htb', required=True)
    parser.add_argument('-n', action='store', metavar='Nameserver', type=str, help='Nameservers separated by a comma.\tExample: ns1.inlanefreight.htb,ns2.inlanefreight.htb')
    parser.add_argument('-v', action='version', version='DNS-AXFR - v1.0', help='Prints the version of DNS-AXFR.py')

    # Assign given arguments
    args = parser.parse_args()

    # Variables
    Domain = args.d
    NS.nameservers = list(args.n.split(","))

    # Check if URL is given
    if not args.d:
        print('[!] You must specify target Domain.\n')
        print(parser.print_help())
        exit()

    if not args.n:
        print('[!] You must specify target nameservers.\n')
        print(parser.print_help())
        exit()

    # For each nameserver
    for nameserver in NS.nameservers:

        # Try AXFR
        AXFR(Domain, nameserver)

    # Print the results
    if Subdomains is not None:
        print('-------- Found Subdomains:')

        # Print each subdomain
        for subdomain in Subdomains:
            print('{}'.format(subdomain))

    else:
        print('No subdomains found.')
        exit()
```

First of all, we can give this script the appropriate privileges necessary to run it independently. If we now execute this script without passing the needed arguments, we get an error, which tells us which arguments are necessary.

#### DNS-AXFR.py - Execution without arguments

Argparse

```shell-session
gdxqpardo@htb[/htb]$ chmod +x dns-axfr.py
gdxqpardo@htb[/htb]$ ./dns-axfr.py

usage: dns-axfr.py [options] -d <DOMAIN>
dns-axfr.py: error: the following arguments are required: -d
```

So when we test if we have set everything and do not give the script any arguments, we should get the error message as we expected. Next, we can check the `help` function by using "`-h`" as an argument.

#### DNS-AXFR.py - Help Message

Argparse

```shell-session
gdxqpardo@htb[/htb]$ ./dns-axfr.py -h

usage: dns-axfr.py [options] -d <DOMAIN>

optional arguments:
  -h, --help     show this help message and exit
  -d Domain      Target Domain. Example: inlanefreight.htb
  -n Nameserver  Nameservers separated by a comma. Example:
                 ns1.inlanefreight.htb,ns2.inlanefreight.htb
  -v             Prints the version of DNS-AXFR.py

DNS Zonetransfer Script
```

Now we can also check if the `version` for this script is displayed as desired.

#### DNS-AXFR.py - Version

Argparse

```shell-session
gdxqpardo@htb[/htb]$ ./dns-axfr.py -v

DNS-AXFR - v1.0
```

After we have verified that our script works, we can deploy it and test it on our target domain. In this example, our target domain is "inlanefreight.com."

#### DNS-AXFR.py - Example

Argparse

```shell-session
gdxqpardo@htb[/htb]$ ./dns-axfr.py -d inlanefreight.com -n ns1.inlanefreight.com,ns2.inlanefreight.com

[*] Successful Zone Transfer from ns1.inlanefreight.com
[*] Successful Zone Transfer from ns2.inlanefreight.com
-------- Found Subdomains:
adm.inlanefreight.com
blog.inlanefreight.com
wlan.inlanefreight.com
afdc0102.inlanefreight.com
autodiscover.inlanefreight.com
kfdcex07.inlanefreight.com
<SNIP>
```

