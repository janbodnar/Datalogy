# Tasks to clean data

## Clean data

Remove quotes, commas and spaces from the `unclean.txt` and organize the words  
into one column  
  
- VIM  
`:%s/[",]//g` - remove " and , chars  
`:%s/\s\+/\r/g` - split words into column  
`:g/^$/d` - delete blank lines   

- VS Code 
use Ctrl + H to remove chars; use regex option  
`\s+` -> `\n`  
`\n\n` -> `\n`  

- Linux tr command

```console 
$ tr -d '",' < unclean.txt | tr -s '[:space:]' '\n'
```  
-d option deletes a set of characters; the -s (for squeeze) replaces multiple occurrences    

- Python solution

```python
#!/usr/bin/python

import sys
import re

filename = sys.argv[1]

with open(filename, 'r') as f:

    data = f.read()
    cleaned = re.sub(r'[,"]', '', data) # remove punctuation marks

    data2 = re.split("\s+", cleaned)
    words = [e for e in data2 if e] # remove empty elements

    for word in words:
        print(word)
```

- Perl solution 

```perl
#!/usr/bin/perl 

use v5.30;
use warnings;

for (<>) {

    $_ =~ s/[",]//g;
    $_ =~ s/\s+/\n/g;

    print $_;
}
```

```console 
$ ./clean_data.pl uncleant.txt
```

- F# solution  

```F#
open System.IO
open System
open System.Text.RegularExpressions

let args = fsi.CommandLineArgs.[1..] 

let data = File.ReadAllText args.[0]

let replaced = Regex.Replace(data, "[\",]", "")

Regex.Split(replaced, "\s+")
|> Array.iter
    (fun e ->
        if (not (String.IsNullOrWhiteSpace(e))) then
            Console.WriteLine(e))
```

```console
$ dotnet fsi clean_words.fsx unclean.txt
```
