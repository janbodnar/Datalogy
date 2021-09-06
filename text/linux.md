# Linux commands

`$ wc -w thermopylae.txt` - # of words  
`$ wc -l thermopylae.txt` - # of lines  
`$ tr ' ' '\n' < thermopylae.txt | grep of | wc -l` - # of  
`$ cat thermopylae.txt | tr -d [:punct:] | tr -s [:space:] '\n' | grep -wc -e '...'` - # of three-letter words  
`$ cat thermopylae.txt | tr -d [:punct:] | tr -s [:space:] '\n' | sort | uniq | wc -l` - # of unique words  
`$ cat thermopylae.txt | tr -dc [AEIUOaeiou] | wc -c`  - # of vowels  
`$ cat thermopylae.txt | tr -d [:punct:][:space:][AEIOUaeiou] | wc -c` - # of consonants  
`$ cat thermopylae.txt | tr -dc '[:punct:]' | wc -c` - # of punctuation marks  
`$ cat thermopylae.txt | tr -s [:space:] '\n' | grep -wc -e '[A-Z].*'` - # of capitalized words  
`$ cat thermopylae.txt | tr -s [:space:] '\n' |  grep -wc -e '[ob].*'` - # starting with o or b  
`$ cat thermopylae.txt | tr -s [:space:] '\n' |  grep -wic -e '[ob].*'` - # starting with o or b, ci  
`$ cat thermopylae.txt | tr -s [:space:] '\n' |  grep -wc -e '.*e'` - # of words ending in 'e'  
