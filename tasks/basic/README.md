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


