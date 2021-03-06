# Merge files 

Join words from words.txt & adjectives.txt 
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

- Python solutions

Example I

```python
#!/usr/bin/python

import sys

from pathlib import Path
 
fname1 = sys.argv[1]
fname2 = sys.argv[2]

lines1 = Path(fname1).read_text(encoding='utf-8').splitlines()
lines2 = Path(fname2).read_text(encoding='utf-8').splitlines()

for e in zip(lines1, lines2):
    print(' '.join(e))
```

Example II

```python
#!/usr/bin/python

import sys

from pathlib import Path
 
fname1 = sys.argv[1]
fname2 = sys.argv[2]

p1 = Path(fname1).open()
p2 = Path(fname2).open()

lines = [[line.rstrip() for line in f] for f in (p1, p2)]
data = list(zip(lines[0], lines[1]))

for e in data:
    print(' '.join(e))
    
# data = [map(str.rstrip, f.readlines()) for f in (p1, p2)]
# data2 = list(zip(data[0], data[1]))

# for e1, e2 in data2:
#    print(f'{e1} {e2}')    
```

Example III

```python
#!/usr/bin/python

import sys

fname1 = sys.argv[1]
fname2 = sys.argv[2]

with open(fname1) as f: lines1 = list(f) 
with open(fname2) as f: lines2 = list(f) 

data = list(zip(lines1, lines2))
cleaned = [f'{e1.rstrip()} {e2.rstrip()}' for e1, e2 in data]

for e in cleaned:
    print(e)    
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
    say $joined if !($joined =~ /^\s*$/);
    # "$joined\n" >> io('combined2.txt');
}
```

- Go solution 

```Go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"strings"
)

func main() {

	fname1 := "adjectives.txt"
	fname2 := "words.txt"

	bdata1, err := ioutil.ReadFile(fname1)

	if err != nil {

		log.Fatal(err)
	}

	bdata2, err := ioutil.ReadFile(fname2)

	if err != nil {

		log.Fatal(err)
	}

	data1 := string(bdata1)
	data2 := string(bdata2)

	lines1 := strings.Split(data1, "\n")
	lines2 := strings.Split(data2, "\n")

	for i := 0; i < len(lines1); i++ {

		res := fmt.Sprintf("%s %s", lines1[i], lines2[i])

		if len(strings.TrimSpace(res)) > 0 {
			fmt.Println(res)
		}
	}
}
```

- C# solution 

```C#
using System;
using System.Linq;
using System.IO;

var lines1 = File.ReadAllLines(args[0]);
var lines2 = File.ReadAllLines(args[1]);

var res = lines1.Zip(lines2, (x, y) => $"{x} {y}");

foreach (var e in res)
{
    if (!string.IsNullOrWhiteSpace(e))
    {
        Console.WriteLine(e);
    }
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
