# F# 

## Count words & unique words

```F#
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

```F#
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

```F#
open System.IO
open System.Text.RegularExpressions

let args = fsi.CommandLineArgs.[1..]

let fileName = args.[0]
let word = args.[1]

let data = File.ReadAllText(fileName)
let n = (data, @$"\b{word}\b") |> Regex.Matches |> Seq.length

printfn $"# of word {word} {n}"
```

## Count three-letter words

```F#
open System.IO
open System.Text.RegularExpressions

let args = fsi.CommandLineArgs.[1..]

let fileName = args.[0]

let data = File.ReadAllText(fileName)
let n = (data, @"\b\w{3}\b") |> Regex.Matches |> Seq.length

printfn $"# of three-letter words {n}"
```

## Count vowels & consonants  

```F#
open System.IO
open System.Text.RegularExpressions

let args = fsi.CommandLineArgs.[1..]

let fileName = args.[0]

let data = File.ReadAllText(fileName)

let n1 =
    (data, "(?i)[aeiou]")
    |> Regex.Matches
    |> Seq.length

printfn $"# of vowels: {n1}"

let n2 =
    (data, "(?i)[bcdfghjklmnpqrstvwxyz]")
    |> Regex.Matches
    |> Seq.length

printfn $"# of consonants: {n2}"
```

## Count punctuation marks  

```F#
open System.IO
open System.Text.RegularExpressions

let args = fsi.CommandLineArgs.[1..]

let fileName = args.[0]

let data = File.ReadAllText(fileName)

let n =
    (data, "\p{P}")
    |> Regex.Matches
    |> Seq.length

printfn $"# of punctuation marks: {n}"
```

## Count capitalized words

```F#
open System.IO
open System.Text.RegularExpressions

let args = fsi.CommandLineArgs.[1..]

let fileName = args.[0]

let data = File.ReadAllText(fileName)

let n =
    (data, "[A-Z]\w*")
    |> Regex.Matches
    |> Seq.length

printfn $"# of capitalized words: {n}"
```
## Count words starting in o/b (ci)

```F#
open System.IO
open System.Text.RegularExpressions

let args = fsi.CommandLineArgs.[1..]

let fileName = args.[0]

let data = File.ReadAllText(fileName)

let n1 =
    (data, @"\b[ob]\w+\b")
    |> Regex.Matches
    |> Seq.length

printfn $"# of words starting in o/b: {n1}"

let n2 =
    (data, @"(?i)\b[ob]\w+\b")
    |> Regex.Matches
    |> Seq.length

printfn $"# of words starting in o/b or O/B: {n2}"
```
