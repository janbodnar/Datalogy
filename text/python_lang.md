# Python


## Words and unique words

```python
#!/usr/bin/python

import sys
import re
from collections import Counter

filename = sys.argv[1]

with open(filename, 'r') as f:
    
    data = f.read()
    cleaned = re.sub("[,.?!]", '', data) # remove punctuation marks
    
    data2 = re.split("\s+", cleaned)
    words = [e for e in data2 if e] # remove empty elements
    
    n1 = len(words)
    n2 = len(Counter(words))

    print(f'{n1} words')
    print(f'{n2} unique words')
```

## Count lines

```python
#!/usr/bin/python

import sys

filename = sys.argv[1]

with open(filename, 'r') as f:
    
    lines = f.readlines()
    cleaned = [e for e in lines if e.strip()] # remove blank lines

    n = len(cleaned)

    print(f'# of lines: {n}')
````

## Count word occurrences

```python
#!/usr/bin/python

import sys
import re

filename = sys.argv[1]
word = sys.argv[2]

with open(filename, 'r') as f:
    
    data = f.read()

    found = re.findall(fr"\b{word}\b", data, re.IGNORECASE)
    n = len(found)
    
    print(f'# of occurences of word {word}: {n}')
```

## Count three-letter words

```python
#!/usr/bin/python

import sys
import re

filename = sys.argv[1]

with open(filename, 'r') as f:
    
    data = f.read()

    found = re.findall(r"\b\w{3}\b", data, re.IGNORECASE)
    n = len(found)
    
    print(f'# of three-letter words: {n}')
```

## Count vowels & consonants 

```python
#!/usr/bin/python

import sys
import re

filename = sys.argv[1]

with open(filename, 'r') as f:
    
    data = f.read()

    found = re.findall(r"[aeiou]", data, re.IGNORECASE)
    n = len(found)
    
    print(f'# of vowels: {n}')

    found = re.findall(r"[bcdfghjklmnpqrstvwxyz]", data, re.IGNORECASE)
    n = len(found)
    
    print(f'# of consonants: {n}')
```


## Count punctuation marks

```python
#!/usr/bin/python

import sys
import re

filename = sys.argv[1]

with open(filename, 'r') as f:
    
    data = f.read()

    found = re.findall(r"[,.?!\-\"']", data)
    n = len(found)
    
    print(f'# of punctuation marks: {n}')
```

## Count capitalized words

```python
#!/usr/bin/python

import sys
import re

filename = sys.argv[1]

with open(filename, 'r') as f:
    
    data = f.read()

    found = re.findall(r"[A-Z]\w*", data)
    n = len(found)
    
    print(f'# of capitalized words: {n}')
```

## Count words starting in o/b (ci)

```python
#!/usr/bin/python

import sys
import re

filename = sys.argv[1]

with open(filename, 'r') as f:
    
    data = f.read()

    found = re.findall(r"\b[ob]\w*\b", data)
    n = len(found)

    print(f'# of words starting in o/b: {n}')

    found = re.findall(r"\b[ob]\w*\b", data, re.I)
    n = len(found)

    print(f'# of words starting in o/b or O/B: {n}')
```

## Count words ending in e

```python
#!/usr/bin/python

import sys
import re

filename = sys.argv[1]

with open(filename, 'r') as f:
    
    data = f.read()

    found = re.findall(r"\b\w*e\b", data)
    n = len(found)
    
    print(f'# of words ending in e: {n}')
```
