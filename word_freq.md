# Word frequency

## AWK 

```awk
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

### Example I

```python
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

### Example II
 
```python
#!/usr/bin/python

from itertools import (groupby, starmap)
from operator import itemgetter
from pathlib import Path
import re

def most_frequent_words(filepath, *, encoding='utf-8'):
    """
    A list of word-frequency pairs sorted by their occurrences.
    The words are read from the given file.
    """
    def word_and_frequency(word, words_group):
        return word, capacity(words_group)
 
    file_contents = filepath.read_text(encoding=encoding)
    # words = file_contents.lower().split()
    words = re.split(r"\W+", file_contents)
    grouped_words = groupby(sorted(words))
    
    # print(type(grouped_words))

    # for word in grouped_words:
    #     print(list(word))

    for key, group in grouped_words:
        key_and_group = {key : list(group)}
        print(key_and_group)

    words_and_frequencies = starmap(word_and_frequency, grouped_words)

    return sorted(words_and_frequencies, key=itemgetter(1), reverse=True)
 
 
def capacity(iterable):
    """Returns a number of elements in an iterable"""
    return sum(1 for _ in iterable)
 
filename = Path('the-king-james-bible.txt')
n = 70

words_and_counts = most_frequent_words(filename)

for w, i in words_and_counts[:n]:
    print(f'{w} {i}')

# print(*words_and_counts[:n], sep='\n')
```
 
## Perl

```perl
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

### Example I

```C#
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

### Example II

```C#
using System.IO;
using System.Linq;
using System.Text.RegularExpressions;

var fileName = "the-king-james-bible.txt";
var text = File.ReadAllText(fileName);

var dig = new Regex(@"\d");
var matches = new Regex("[a-z-A-Z']+").Matches(text);

var words = 
    (from match in matches
        let val = match.Value
        where !dig.IsMatch(val)
    select match.Value).ToList();

var topTen =
    (from word in words
        group word by word into wg
        orderby wg.Count() descending
        select new {word = wg.Key, Total = wg.Count()}
    ).Take(10);


foreach (var e in topTen)
{
    Console.WriteLine($"{e.word}: {e.Total}");
}
```

## F# 

```F#
open System
open System.IO
open System.Text.RegularExpressions

let fileName = "the-king-james-bible.txt"
let data = File.ReadAllText(fileName)

let dig = Regex(@"\d")
let rx = Regex("[a-z-A-Z']+")

let matches = rx.Matches(data)

let topTen =
    matches
    |> Seq.map (fun m -> m.Value)
    |> Seq.filter (dig.IsMatch >> not)
    |> Seq.countBy id
    |> Seq.sortByDescending snd
    |> Seq.take 10

topTen
|> Seq.iter (fun (e, n) -> Console.WriteLine($"{e}: {n}"))
```

# Go 

## Example I

```Go
package main

import (
    "fmt"
    "io/ioutil"
    "log"
    "sort"
    "strings"
)

func main() {

    fileName := "the-king-james-bible.txt"

    bs, err := ioutil.ReadFile(fileName)

    if err != nil {

        log.Fatal(err)
    }

    text := string(bs)

    fields := strings.FieldsFunc(text, func(r rune) bool {

        return !('a' <= r && r <= 'z' || 'A' <= r && r <= 'Z' || r == '\'')
    })

    wordsCount := make(map[string]int)

    for _, field := range fields {

        wordsCount[field]++
    }

    keys := make([]string, 0, len(wordsCount))

    for key := range wordsCount {

        keys = append(keys, key)
    }

    sort.Slice(keys, func(i, j int) bool {

        return wordsCount[keys[i]] > wordsCount[keys[j]]
    })

    for idx, key := range keys {

        fmt.Printf("%s %d\n", key, wordsCount[key])

        if idx == 10 {
            break
        }
    }
}
```

## Example II

```Go
package main

import (
    "fmt"
    "io/ioutil"
    "log"
    "regexp"
    "sort"
)

type WordFreq struct {
    word string
    freq int
}

func (p WordFreq) String() string {
    return fmt.Sprintf("%s %d", p.word, p.freq)
}

func main() {

    fileName := "the-king-james-bible.txt"

    reg := regexp.MustCompile("[a-zA-Z']+")
    bs, err := ioutil.ReadFile(fileName)

    if err != nil {
        log.Fatal(err)
    }

    text := string(bs)
    matches := reg.FindAllString(text, -1)

    words := make(map[string]int)

    for _, match := range matches {
        words[match]++
    }

    var wordFreqs []WordFreq
    for k, v := range words {
        wordFreqs = append(wordFreqs, WordFreq{k, v})
    }

    sort.Slice(wordFreqs, func(i, j int) bool {

        return wordFreqs[i].freq > wordFreqs[j].freq
    })

    for i := 0; i < 10; i++ {

        fmt.Println(wordFreqs[i])
    }
}
```



## Raku

```raku
#!/usr/bin/raku

#say "the-king-james-bible.txt".IO.slurp.words.raku;

my $text = "the-king-james-bible.txt".IO.slurp;
my %seen;

#say $text;

%seen{$_}++ for $text.comb(/<[a..zA..Z']>+/);

#for %seen {
#    .say;
#}

for %seen.sort(-*.value)[^10] {

    .say;
}

exit 0;

for %seen.sort(-*.value) {
  
    say $_;
}

#say %sorted.head(10);

exit 0;

my $i;
for %seen.sort({-.value}) {
    #say "{$_.key} {$_.value}";
    say $_;

    $i++;
    last if $i == 10;
}

#my %sorted = sort {%seen{$^a} <=> %seen{$^b}}, %seen.keys;

#for %sorted -> $e {

 #   say "$e => {%sorted{$e}}" 
#}
```
