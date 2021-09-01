# Tasks to clean data

## Clean data

Remove quotes, commas and spaces from the `unclean.txt` and organize the words  
into one column  
  
1) VIM  
`:%s/[",]//g` - remove " and , chars  
`:%s/\s\+/\r/g` - split words into column  
`:g/^$/d` - delete blank lines   

2) VS Code 
use Ctrl + H to remove chars; use regex option 
`\s+` -> `\n`
`\n\n` -> `\n`
