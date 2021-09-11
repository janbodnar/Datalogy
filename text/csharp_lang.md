# C#

## Count words & unique words

```C#
using System;
using System.Linq;
using System.IO;
using System.Text.RegularExpressions;

var fileName = args[0];
var data = File.ReadAllText(fileName);

var cleaned = Regex.Replace(data, @"\p{P}*", ""); 
var words = Regex.Split(cleaned, @"\s+");

var res = from word in words 
    where !String.IsNullOrWhiteSpace(word)
    select word;

var n1 = res.Count();
var n2 = res.Distinct().Count();

Console.WriteLine($"# of words: {n1}");
Console.WriteLine($"# of unique words: {n2}");
```

\p{P} - punctuation character class    


## Count lines 

```C#
using System;
using System.Linq;
using System.IO;
using System.Text.RegularExpressions;

var fileName = args[0];
var lines = File.ReadAllLines(fileName);

var n = (from line in lines
    where !String.IsNullOrWhiteSpace(line)
    select line).Count();

Console.WriteLine($"# of lines: {n}");
```


## Count word occurrence 

```C#
using System;
using System.Linq;
using System.IO;
using System.Text.RegularExpressions;

var fileName = args[0];
var word = args[1];

var data = File.ReadAllText(fileName);
var rx = new Regex(@$"\b{word}\b", RegexOptions.Compiled);

var matches = rx.Matches(data);

Console.WriteLine($"# of word {word}: {matches.Count}");
```

## Count three-letter words 

```C#
using System;
using System.Linq;
using System.IO;
using System.Text.RegularExpressions;

var fileName = args[0];
var word = args[1];

var data = File.ReadAllText(fileName);
var rx = new Regex(@"\b\w{3}\b", RegexOptions.Compiled);

var matches = rx.Matches(data);

foreach (Match match in matches)
{
    Console.WriteLine(match);
}

Console.WriteLine($"# of three-letter words: {matches.Count}");
```

## Count vowels & consonants 

```C#
using System;
using System.Linq;
using System.IO;
using System.Text.RegularExpressions;

var fileName = args[0];
var data = File.ReadAllText(fileName);

var rx1 = new Regex(@"(?i)[aeiou]", RegexOptions.Compiled);

var n1 = rx1.Matches(data).Count();
Console.WriteLine($"# of vowels: {n1}");


var rx2 = new Regex(@"(?i)[bcdfghjklmnpqrstvwxyz]", RegexOptions.Compiled);

var n2 = rx2.Matches(data).Count();
Console.WriteLine($"# of consonants: {n2}");
```

## Count punctuation marks 

```C#
using System;
using System.Linq;
using System.IO;
using System.Text.RegularExpressions;

var fileName = args[0];
var data = File.ReadAllText(fileName);

var rx = new Regex(@"\p{P}", RegexOptions.Compiled);

var n = rx.Matches(data).Count();
Console.WriteLine($"# of punctunation marks: {n}");
```

