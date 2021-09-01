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

3) Perl solution 

`$ ./clean_data.pl uncleant.txt`  

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

3) F# solution  

```
open System.IO
open System
open System.Text.RegularExpressions

let data = File.ReadAllText "unclean.txt"

let replaced = Regex.Replace(data, "[\",]", "")

Regex.Split(replaced, "\s+")
|> Array.iter
    (fun e ->
        if (not (String.IsNullOrWhiteSpace(e))) then
            Console.WriteLine(e))
```
