### Basic tasks

## Join words from words.txt & adjectives.txt 
into one file/output; join words from the same lines

- Linux commands

```
$ paste adjectives.txt words.txt | column -t
blue    sky
brown   cup
strong  wind
early   snow
old     tree
```
`paste`  writes lines consisting of the sequentially corresponding lines
from each FILE, separated by TABs, to standard output.  
The `column` utility formats its input into multiple columns  
