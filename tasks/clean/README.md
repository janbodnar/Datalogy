# Tasks to clean data

## Clean data

Remove quotes, commas and spaces from the `unclean.txt` and organize the words  
into one column  
  
1) VIM  
`:%s/[",]//g` - remove " and , chars  
`:%s/\s\+/\r/g` - split words into column  
`:g/^$/d` - delete blank lines   

2) VS Code 
use Ctrl + H to remove chars; use regex option  
`\s+` -> `\n`  
`\n\n` -> `\n`  

3) Linux commands 

`tr -d '",' < unclean.txt | tr -s '[:space:]' '\n'`  - one liner with tr command  

4) Perl solution 

```
#!/usr/bin/perl 

use v5.30;
use warnings;

for (<>) {

    $_ =~ s/[",]//g;
    $_ =~ s/\s+/\n/g;

    print $_;
}
```
`$ ./clean_data.pl uncleant.txt`  

5) F# solution  

```
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

`dotnet fsi clean_words.fsx unclean.txt`
