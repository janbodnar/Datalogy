# F# 

## Count words & unique words

```
open System
open System.IO
open System.Text.RegularExpressions

let args = fsi.CommandLineArgs.[1..]

let fileName = args.[0]

let data = File.ReadAllText(fileName)

let cleaned = Regex.Replace(data, "\p{P}*", "") // \p{P} - punctuation character class  

let words =
    (cleaned, "\s+")
    |> Regex.Split
    |> Array.filter (fun e -> not (String.IsNullOrWhiteSpace(e)))

let n1 = words |> Array.length
printfn "# of words: %A" n1

let n2 = words |> Array.countBy id |> Array.length
printfn "# of unique words: %A" n2
```

## Count lines

```
open System
open System.IO

let args = fsi.CommandLineArgs.[1..]
let fileName = args.[0]

let lines = File.ReadAllLines(fileName)

let n =
    lines
    |> Array.filter (String.IsNullOrWhiteSpace >> not)
    |> Array.length

printfn "# of lines: %d" n
```

## Count word occurrence 

```
open System.IO
open System.Text.RegularExpressions

let args = fsi.CommandLineArgs.[1..]

let fileName = args.[0]
let word = args.[1]

let data = File.ReadAllText(fileName)
let n = (data, @$"\b{word}\b") |> Regex.Matches |> Seq.length

printfn $"# of word {word} {n}"
```
