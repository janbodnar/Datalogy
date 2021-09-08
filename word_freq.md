# Word frequency

## AWK 

```
    {  

        if (NR == 1) { 
            sub(/^\xef\xbb\xbf/,"")
        }

        gsub(/[,;!()*:?.]*/, "")
    
        for (i = 1; i <= NF; i++) {
    
            if ($i ~ /^[0-9]/) { 
                continue
            }
    
            w = $i
            words[w]++
        }
    } 
    
    END {
    
        for (idx in words) {
    
            print idx, words[idx]
        }
    }
```

## Python

```
#!/usr/bin/python

import collections
import re

filename = 'the-king-james-bible.txt'

def get_words():

    words = []

    with open(filename) as f:

        for line in f:

            fields = re.split(r"\W+", line)

            for w in fields:

                if w and not w.isdigit():
                    words.append(w)

    return words

words = get_words()
c = collections.Counter(words)

top_ten = c.most_common(10)

for e, i in top_ten:
    print(f'{e}: {i}')
``` 
 
## Perl

```
#!/usr/bin/perl

use v5.30;
use strict;
use warnings;

use Path::Tiny qw( path );
 
my $file_name = 'the-king-james-bible.txt';
my $data = path($file_name)->slurp_utf8;

my @matches = $data =~ /[a-zA-Z']+/g;
my %seen = ();

foreach my $word (@matches) {

    $seen{$word}++;
}

my @sorted_data = sort { $seen{$b} <=> $seen{$a} } keys %seen;

foreach my $word (@sorted_data) {

    say "$word $seen{$word}";
}
```

## C#

```
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text.RegularExpressions;

var fileName = "the-king-james-bible.txt";
var text = File.ReadAllText(fileName);


var match = Regex.Match(text, @"\w+");
var freq = new Dictionary<string, int>();

while (match.Success)
{
    string word = match.Value;

    if (word.All(char.IsDigit))
    {
        match = match.NextMatch();
        continue;
    }

    if (freq.ContainsKey(word))
    {
        freq[word]++;
    }
    else
    {
        freq.Add(word, 1);
    }

    match = match.NextMatch();
}

Console.WriteLine("Rank  Word  Frequency");
Console.WriteLine("====  ====  =========");
int rank = 1;

foreach (var elem in freq.OrderByDescending(a => a.Value).Take(90))
{
    Console.WriteLine("{0,2}    {1,-5}   {2,8}", rank++, elem.Key, elem.Value);
}
```
