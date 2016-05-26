# grep-sed-awk-magic 
Magic tricks using grep, sed and awk
grep magic

## grep a tailed file
`tail -F ${filename} | grep --line-buffered ${search_term}`

## remove blank/empty lines from output with grep
`cat $file | grep -v ^$`

## list the name of the files that have matches in grep
`find . -name "${target_filetype}" | xargs grep -l ${search_term}`
