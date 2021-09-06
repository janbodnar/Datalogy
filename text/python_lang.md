# Python


## Words and unique words

```
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

```
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

```
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

```
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

## Count capitalized words

```
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

## Count words ending in e

```
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
