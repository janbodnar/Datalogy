# Tasks to clean data

## Clean data

Remove quotes, commas and spaces from the `unclean.txt` and organize the words  
into one column  
  
a) VIM 
`:%s/[",]//g` - remove " and , chars  
`:%s/\s\+/\r/g` - split words into column
`:g/^$/d` - delete blank lines   
