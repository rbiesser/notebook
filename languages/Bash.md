Bash
===

Expansion
---

### Wildcards

Wildcard | Meaning
---|---
\* | Matches any character
\? | Matches any single character
[characters] | Matches any of a set of characters<br>[[:alnum:]] alphanumeric characters<br>[[:alpha:]] Alphabetic characters</br>[[:digit:]] Numerals<br>[[:upper:]] Uppercase alphabetic characters<br>[[:lower:]] Lowercase alphabetic characters
[!characters] | Matches any character not in the set
~ | Home directory of the user

### Arithmetic Expansion
`$ echo $((expression))` - Use with the math operators [ * / + - % ] and nestable.
```
$ echo $((2 + 2))
4
$ echo $(($((2 + 2)) % 2))
0
$ echo $(((2 + 2) %2))
0
```

### Brace Expansion
`$ echo {pattern1,pattern2}` - Will run for every pattern in the braces and nestable.  
`$ echo {0..9}` or `$ echo {A..Z}` - A range either increment or decrement from first to last.

### Command Substition
`$ ls -l $(which cp)` - Uses the output of the command in parenthesis.  
``$ ls -l `which cp`` ` - Older version with backticks.

### Quotes
Use double quotes to suppress word-splitting, pathname expansion, tilde expansion, and brace expansion. Parameter expansion, arithmetic expansion, and command substitution are still carried out.  
Use singles quotes to suppress all expansions.
```
$ echo "text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER"
text ~/*.txt {a,b} foo 4 me
$ echo 'text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER'
text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER
```

### Escape Characters
Escape character | Meaning
---|---
\n | newline - move cursor to next line and reset cursor to beginning
\t | tab - move cursor to next horizontal tab stop
\a | alert - make terminal beep
\\\ | backslash - escape the escape character
\f | formfeed - move down a line keeping cursor position
\  | escape the space character

Check if a file exists
---
Use any of these snippets to check that a FILE variable exists:
```bash
# using test expression
FILE=/etc/resolv.conf
if test -e "$FILE"; then
    echo "$FILE exist"
fi

# single brackets
FILE=/etc/resolv.conf
if [ -e "$FILE" ]; then
    echo "$FILE exist"
fi

# double brackets
FILE=/etc/resolv.conf
if [[ -e "$FILE" ]]; then
    echo "$FILE exist"
fi

# or as one line
test -e /etc/resolv.conf && echo "$FILE exist"
[ -e /etc/resolv.conf ] && echo "$FILE exist"
[[ -e /etc/resolv.conf ]] && echo "$FILE exist"
```
There are several test conditions that can be done by using different flags. The most common would be `-e` to check if the file exists, `-f` to check if the file exists and is a regular file, or `-d` to check if the file exists and is a directory. See https://linuxize.com/post/bash-check-if-file-exists/#file-test-operators for a more complete list.

References
---
http://linuxcommand.org/lc3_lts0080.php

http://www.bashoneliners.com/

https://linuxize.com/tags/bash/
