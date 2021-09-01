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

- F# solutions

```
open System.IO
open System

let args = fsi.CommandLineArgs.[1..] 

let lines1 = File.ReadAllLines(args.[0])
let lines2 = File.ReadAllLines(args.[1])

let zipped = Array.zip lines1 lines2

for (w1, w2) in zipped do
    printfn $"{w1} {w2}"
```

```
$ dotnet fsi combine_lines.fsx adjectives.txt words.txt
blue sky
brown cup
strong wind
early snow
old tree
```

Writes to standard output; could be redirected if necessary 

```
open System.IO

let args = fsi.CommandLineArgs.[1..] 

let lines1 = File.ReadAllLines(args.[0])
let lines2 = File.ReadAllLines(args.[1])

let data =
    Array.map (fun (e1, e2) -> $"{e1} {e2}") (Array.zip lines1 lines2)

File.WriteAllLines("combined.txt", data)
```

Writes output to file.
