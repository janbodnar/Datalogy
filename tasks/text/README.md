# Text processing

## Counting words in small text

- \# of words 
- \# of lines
- \# 'of'
- \# of words 'and', 'the', 'by'
- \# of three-letter words
- \# of unique words
- \# of vowels and consonants
- \# of punctuation marks 
- \# of capitalized words
- \# of words that start with either 'o', or 'b'
- \# of words that start with either 'o', or 'b', case insensitive
- \# of words that end with 'e'

### Linux commands

`$ wc -w thermopylae.txt` - # of words  
`$ wc -l thermopylae.txt` - # of lines  
`$ tr ' ' '\n' < thermopylae.txt | grep of | wc -l` - # of  
`$ cat thermopylae.txt | tr -d [:punct:] | tr -s [:space:] '\n' | grep -wc -e '...'` - # of three-letter words  
`$ cat thermopylae.txt | tr -d [:punct:] | tr -s [:space:] '\n' | wc -l` - # of unique words  
`$ cat thermopylae.txt | tr -dc [AEIUOaeiou] | wc -c`  - # of vowels  
`$ cat thermopylae.txt | tr -d [:punct:][:space:][AEIOUaeiou] | wc -c` - # of consonants  
`$ cat thermopylae.txt | tr -dc '[:punct:]' | wc -c` - # of punctuation marks  
`$ cat thermopylae.txt | tr -s [:space:] '\n' | grep -wc -e '[A-Z].*'` - # of capitalized words  
`$ cat thermopylae.txt | tr -s [:space:] '\n' |  grep -wc -e '[ob].*'` - # starting with o or b  
`$ cat thermopylae.txt | tr -s [:space:] '\n' |  grep -wic -e '[ob].*` - # starting with o or b, case insen  
`$ cat thermopylae.txt | tr -s [:space:] '\n' |  grep -wc -e '.*e'` - # of words ending in 'e'  

### Perl programs

```
#!/usr/bin/perl 

use v5.30;
use warnings;
use IO::All;

my $fname = shift or die 'provide a filename';

my @lines = io($fname)->chomp->slurp;
@lines = grep { $_ ne '' } @lines; # remove empty elements

my $n = 0;

for my $line (@lines) {

    $line =~ s/[[:punct:]]//g;

    my @words = split " ", $line;
    $n += @words;
}


my $n_lines = @lines;

say "There are $n words and $n_lines lines";
```

Counts words and lines

```
#!/usr/bin/perl 

use v5.30;
use warnings;
use IO::All;

my $fname = shift or die 'provide a filename';

my $contents = io($fname)->all;
my %res;

my $cleaned = $contents =~ s/[[:punct:]]//gr;
my @words = split " ", $cleaned;

for my $word (@words) {

     ++$res{lc $word};
}

my $n = keys %res;

say "There are $n unique words";
```

Counts unique words, case insensitively

```
#!/usr/bin/perl 

use v5.30;
use warnings;
use IO::All;

my $fname = shift or die 'provide a filename';

my $contents = io($fname)->all;

# Perl transliteration operator does not support 
# character classes
my $n1 = $contents =~ y/,.?!-"'//; 
say "$n1 punctuation marks";

my $n2 = $contents =~ y/aeiouAEIOU//;
say "$n2 vowels";

my $n3 = $contents =~ y/bcdfghjklmnpqrstvwxyzBCDFGHJKLMNPQRSTVWXYZ//;
say "$n3 consonants";
```

Counts punctuation marks, vowels and consonants

```
#!/usr/bin/perl 

use v5.30;
use warnings;
use IO::All;

my $fname = shift or die 'provide a filename';

my $contents = io($fname)->all;
$contents =~ s/[[:punct:]]//g;

my @matches = $contents =~ /(\b\w{3}\b)/g;
say "@{[scalar @matches]} three-letter words";

# alternative 1
# push (@matches, $&) while ($contents =~ /\b\w\w\w\b/g);

# alternative 2
# my @words = split " ", $contents;
# my $n = 0;

# for my $word (@words) {
#     if ($word =~ m/^...$/) {
#         $n++;
#     }
# }

# my $n1 = @words; 

# say "$n three-letter words"
```

Count three-letter words  



