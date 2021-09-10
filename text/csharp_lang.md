# C#

## Count words & unique words

```
using System;
using System.Linq;
using System.IO;
using System.Text.RegularExpressions;

var fileName = args[0];
var data = File.ReadAllText(fileName);

var cleaned = Regex.Replace(data, @"\p{P}*", ""); // \p{P} - punctuation character class  
var words = Regex.Split(cleaned, @"\s+");


var res = from word in words 
    where !String.IsNullOrWhiteSpace(word)
    select word;

var n1 = res.Count();
var n2 = res.Distinct().Count();

Console.WriteLine($"# of words: {n1}");
Console.WriteLine($"# of unique words: {n2}");
```




