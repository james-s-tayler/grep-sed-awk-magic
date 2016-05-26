# grep-sed-awk-magic 
Magic tricks using grep, sed and awk
grep magic

## grep a tailed file
`tail -F ${filename} | grep --line-buffered ${search_term}`

## remove blank/empty lines from output with grep 
`cat $file | grep -v ^$`

## list the name of the files that have matches in grep
`find . -name "${target_filetype}" | xargs grep -l ${search_term}`

**Example**:
`find . -name "*.java" | xargs grep -l hack[y]*`

Lists all `java` files under the current directory that contain the words hack or hacky.

## count global number of matches for a given regex with grep

`find . -name "${target_filetype}" | grep -o "${regex}" | wc -l`

**Example**:
`find . -name "*.java" | xargs grep -o "()" | wc -l`

Count the total number of methods in your source code

## find the column number on which a given character occurs in a line with grep
echo ${string} | grep -o . | grep -n ${character_to_search_for} | sed 's/://'

**Example**:
`echo "one two three four" | grep -o . | grep -n " " | sed 's/://'`

This works because `grep -o .` is matching 'only' 'any character' and returning all matches on a new line.
We then use `grep -n ${character_to_search_for}` to output the line number of lines that match the character we are searching for.
