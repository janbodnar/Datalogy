
# Perl

## Count words and lines

```perl
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

## Count unique words, ci

```perl
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

## Count punctuation marks, vowels & consonants

```perl
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

## Count three-letter words  

```perl
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

## Count capitalized words, words starting in o/b, o/b ci, and words ending in e

```perl
#!/usr/bin/perl 

use v5.30;
use warnings;
use IO::All;

my $fname = shift or die 'provide a filename';

my $contents = io($fname)->all;
$contents =~ s/[[:punct:]]//g;

my @matches = $contents =~ /(\b[A-Z]\w*\b)/g;
my $n = @matches;

say "$n capitalized words";

@matches = $contents =~ /(\b[ob]\w*\b)/g;
$n = @matches;

say "$n words starting in o or b";

@matches = $contents =~ /(\b[ob]\w*\b)/gi;
$n = @matches;

say "$n words starting in o or b, ci";

@matches = $contents =~ /(\b\w*e\b)/g;
$n = @matches;

say "$n words ending in e";
```

