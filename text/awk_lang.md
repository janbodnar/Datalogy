# AWK 

## Count lines and words

```awk
{
    words += NF

    if (NF > 0) {
        lines++
    }
}

END { print lines, words }

```

```console
$ awk -f words_lines.awk thermopylae.txt 
4 38
```

Counts lines and words; it counts non-blank lines  

## Count word occurrences  

```awk
{
    for (i=1; i<=NF; i++) {

        field = $i

        if (match(field, "^" word "$")) {
        #if (match(field, "\\y"word"\\y")) {

            n++
        }
    }
}

END { print n }
```

```console
$ awk -v word=of -f count_word.awk thermopylae.txt 
6
```
Count # of  

## Count three-letter words

```awk
{
    for (i=1; i<=NF; i++) {

        field = $i

        if (match(field, /\y...\y/)) {

            n++
        }
    }
}

END { print n }
```

Counts the # of three-letter words; \y is for word boundary  

## Couns vowels and consonants  

```awk
BEGIN { 
    IGNORECASE=1 
}

{
    vows += gsub(/[aeiou]/,"")
    cons += gsub(/[bcdfghjklmnpqrtsvwxyz]/,"")
}

END {
    printf "vowels: %d, consonants: %d\n", vows, cons
}
```

```console
$ awk -f vows_cons.awk thermopylae.txt 
vowels: 72, consonants: 108
```

alternatively, `IGNORECASE` can be set with `awk -v IGNORECASE=1`  


## Count capitalized, starting & ending

```awk
BEGIN {
    # IGNORECASE=1
    cap = 0
    ob_start = 0
    obi_start = 0
    e_end = 0
}

{
    for (i=1; i<=NF; i++) {

        gsub(/[,;!()*:?.]*/, "")

        field = $i

        if (field ~ /^[A-Z]/) {

            cap++
        }

        if (field ~ /^[o,b]/) {

            ob_start++
        }        

        if (tolower(field) ~ /^[o,b]/) {

            obi_start++
        }

        if (field ~ /e$/) {

            e_end++
        }
    }
}

END {  

    print "Capitalized: ", cap 
    print "Starting in o or b: ", ob_start 
    print "Starting in o or b, ci: ", obi_start
    print "Ending in e: ", e_end 
}
```
Counts capitalized words, words starting in o/b, o/b ci, and words ending in e.   
