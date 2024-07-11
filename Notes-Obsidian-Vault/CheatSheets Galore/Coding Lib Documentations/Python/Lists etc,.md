## Table of Contents

      - [Substrings](#Substrings)
  - [For-Each Loop](#For-Each\Loop)
      - [The For-Each Loop](#The\For-Each\Loop)

Code: python

```python
>>> var = "ABCDEF"
>>> print(var[0], var[1], var[2], var[3], var[4], var[5])
A B C D E F
>>> print(var[-1], var[-2], var[-3], var[-4], var[-5], var[-6])
F E D C B A
```
#lists #indicing

|**Negative Index**|**Index**|**Character**|
| :-: | :-: | :-: |
| `-6` | `0` | A |
| `-5` | `1` | B |
| `-4` | `2` | C |
| `-3` | `3` | D |
| `-2` | `4` | E |
| `-1` | `5` | F |

#### Substrings
#substrings #python #listslicing #lists
Code: python

```python
>>> var = "ABCDEF"
>>> print(var[:2])	# Up to index 2
AB
>>> print(var[2:])	# Ignore everything up to index 2
CDEF
>>> print(var[2:4])	# Everything between index 2 and 4 ("2" is counted)
CD
>>> print(var[-2:])	# Up to negative index 2 (last two characters)
EF
```



## For-Each Loop
#python #foreachloop #loops #syntax
 loop over each element (or value) in a list. Consider the below piece of code where we at first have defined a list and then the loop.
#### The For-Each Loop

Code: python

```python
groceries = ['Walnuts', 'Grapes', 'Bird seeds']

for food in groceries:
    print(f'I bought some {food} today.')
```