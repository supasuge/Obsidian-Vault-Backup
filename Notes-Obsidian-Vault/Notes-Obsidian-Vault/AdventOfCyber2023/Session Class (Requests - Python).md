## Table of Contents

  - [Table of Contents](#Table\of\Contents)
          - [To get public IP Address:](#To\get\public\IP\Address:)
    - [get_html_of(url)](#get_html_of(url))
    - [Adding Click Options](#Adding\Click\Options)

## Table of Contents

 - [To get public IP Address:](#To\get\public\IP\Address:)
    - [get_html_of(url)](#get_html_of(url))
    - [Adding Click Options](#Adding\Click\Options)

Useful when we need to maintain a certain context during our web activity. For ex:
- Keeping track of a range of cookies, use a `Session` object. 
```python
import requests
sess = requests.Session()
```



###### To get public IP Address:
```python
	import requests
	resp = requests.get('http://httpbin.org/ip')
	print(resp.content.decode())
```

  
Installing BeautifulSoup

```shell
$ python3 -m pip install beautifulsoup4
```

### get_html_of(url)
```python
import requests
URL = 'http://TARGET:PORT'

def get_html_of(url):
    resp = requests.get(url)
    if resp.status_code != 200:
        print(f'HTTP Status code of {resp.status_code} returned, but 200 was expected. Exiting...')
        exit(1)
    return resp.content.decode()


print(get_html_of(PAGE_URL))
```

- It's always a good idea to try and list idea's/actions on paper, and then for each action, ask oneself, "How do I do this?" and then write up those steps next to it. 
	- In this case:
		- Find all words on the page, ignoring HTML tags and other metadata
		- Count the occurrence of each word and note it down
		- Sort by occurrence
		- Do something with the most frequently occurring words, e.g., print them.

### Adding Click Options
```python
import click
@click.command()
@click.option('--url', '-u', prompt='Web URL', help='URL of webpage to extract from.')
@click.option('--length', '-l', default=0, help='Minimum word length (default: 0, no limit).')

def main(url, length):
	the_words = get_all_words_from(url)
	
---SNIP---
```




























