# grep-sed-awk-magic 
Magic tricks using `grep`, `sed` and `awk`

## `grep` a tailed file
`tail -F ${filename} | grep --line-buffered ${search_term}`

## remove blank/empty lines from output with `grep`
`cat $file | grep -v ^$`

## list the name of the files that have matches in `grep`
`find . -name "${target_filetype}" | xargs grep -l ${search_term}`

**Example**:
`find . -name "*.java" | xargs grep -l hack[y]*`

Lists all `java` files under the current directory that contain the words hack or hacky.

## count global number of matches for a given regex with `grep`

`find . -name "${target_filetype}" | grep -o "${regex}" | wc -l`

**Example**:
`find . -name "*.java" | xargs grep -o "()" | wc -l`

Count the total number of methods in your source code

## find the column numbers on which a given character occurs in a line with grep
`echo ${string} | grep -o . | grep -n ${character_to_search_for} | sed 's/://'`

**Example**:
`echo "one two three four" | grep -o . | grep -n " " | sed 's/://'`

This works because `grep -o .` is matching 'only' 'any character' and returning all matches on a new line.
We then use `grep -n ${character_to_search_for}` to output the line number of lines that match the character we are searching for.


## Display an entire block of text beginning with a given pattern
`awk ‘/start_pattern/,/stop_pattern/’ file.txt`

**Example**:
Assume we have the following `java` class

        public class Yolo
        {
            private int x;

            /**
            * Constructor for objects of class Yolo
            */
            public Yolo() {
                // initialise instance variables
                x = 0;
            }

            public int sampleMethod(int y) {
                // put your code here
                return x + y;
            }
        }

if we issue the command `cat Yolo.java | awk '/public Yolo\(\)/,/^$/'`
it will output just the text starting from the constructors method signature
to the next empty line it finds, thus printing only the constructor.
        
        public Yolo() {
            // initialise instance variables
            x = 0;
        }

## print the last argument with awk
`awk '{print $NF}'`

**Example**:
`find . -name "*.java" | awk -F/ '{print $NF}'`

Lists just the filenames without paths of all java classes. Find will return a list of the full path to each java file and `-F/` tells `awk` to split the string by `/` and then print the last argument in the resulting array.

## print all columns from the nth to the last with awk
`awk '{$1=""; print $0}' somefile`

will print all but very first column.

`awk '{$1=$2=""; print $0}' somefile`

will print all but two first columns

## match a pattern spanning multiple lines in `sed`
`sed ':a;N;$!ba;s/${find_this}/${replace_with_this}/'` 

This loads the entire text into the pattern space so the entire input can be matched on instead of just line by line

## remove duplicate lines from a file without sorting it
`cat file.txt | awk '!x[$0]++'`

No idea how this one works but it's twice as fast as `sort | uniq`
