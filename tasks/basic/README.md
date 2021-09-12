# Basic tasks

## Count words

Count the number of words in `the-king-james-bible.txt`  

```F#
open System.IO
open System.Text.RegularExpressions;

let data = File.ReadAllText("the-king-james-bible.txt")

let g = Regex("[A-Za-z]+").Matches data

printfn "There are %d words" g.Count
```


## Join words from words.txt & adjectives.txt 
into one file/output; join words from the same lines

- Linux commands

```console
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

- AWK 

```console
$ awk '{printf ("%s ", $0); getline < "adjectives.txt"; print $0 }' words.txt
```

- Raku solution 

```raku
#!/usr/bin/raku

.put for [Z] @*ARGS.map: *.IO.lines;
```

- Python solution

```python
#!/usr/bin/python

import sys

from pathlib import Path
 
fname1 = sys.argv[1]
fname2 = sys.argv[2]

data1 = Path(fname1).read_text(encoding='utf-8').splitlines()
data2 = Path(fname2).read_text(encoding='utf-8').splitlines()

data = list(zip(data1, data2))

for e in data:
    print(' '.join(e))
```


- Perl solution 

```perl
#!/usr/bin/perl 

use v5.30;
use warnings;
use IO::All;
use List::MoreUtils qw(zip6);

my $fname1 = shift or die 'need 2 params';
my $fname2 = shift or die 'need 2 params';

my @lines1 = io($fname1)->chomp->slurp; 
my @lines2 = io($fname2)->chomp->slurp; 

my @zipped = zip6 @lines1, @lines2;

for my $e (@zipped) {

    my $joined = join " ", @{ $e };
    say $joined;
    # "$joined\n" >> io('combined2.txt');
}
```

- F# solutions

```F#
open System.IO

let args = fsi.CommandLineArgs.[1..] 

let lines1 = File.ReadAllLines(args.[0])
let lines2 = File.ReadAllLines(args.[1])

let zipped = Array.zip lines1 lines2

for (w1, w2) in zipped do
    printfn $"{w1} {w2}"
```

```console
$ dotnet fsi combine_lines.fsx adjectives.txt words.txt
blue sky
brown cup
strong wind
early snow
old tree
```

Writes to standard output; could be redirected if necessary 

```F#
open System.IO

let args = fsi.CommandLineArgs.[1..] 

let lines1 = File.ReadAllLines(args.[0])
let lines2 = File.ReadAllLines(args.[1])

let data =
    Array.map (fun (e1, e2) -> $"{e1} {e2}") (Array.zip lines1 lines2)

File.WriteAllLines("combined.txt", data)
```

Writes output to file.
