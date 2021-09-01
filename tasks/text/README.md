# Text processing

## Counting words in small text

- # of words 
- # 'of'
- # of lines
- # of words 'and', 'the', 'by'
- # of three letter words
- # of unique words
- # of vowels and consonants
- # of punctuation marks 
- # of capitalized words
- # of words that start with either 'o', or 'b'
- # of words that start with either 'o', or 'b', case insensitive
- - # of words that end with either 'e'

### Linux commands

`$ wc -l` - # of lines  
`$ tr ' ' '\n' < thermopylae.txt | grep of | wc -l` - # of  
