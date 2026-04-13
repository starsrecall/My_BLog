The Shell Scripting Tutorial
----------------------------

* * *

Loops
-----

Most languages have the concept of loops: If we want to repeat a task twenty times, we don't want to have to type in the code twenty times, with maybe a slight change each time.  
As a result, we have `for` and `while` loops in the Bourne shell. This is somewhat fewer features than other languages, but nobody claimed that shell programming has the power of C.

[](https://www.shellscript.sh/loops.html)

For Loops
---------

`for` loops iterate through a set of values until the list is exhausted:

* * *

[for.sh](https://www.shellscript.sh/eg/for.sh.txt)

```bash
#!/bin/sh
for i in 1 2 3 4 5
do
  echo "Looping ... number $i"
done

```

* * *

Try this code and see what it does. Note that the values can be anything at all:

* * *

[for2.sh](https://www.shellscript.sh/eg/for2.sh.txt)

```bash
#!/bin/sh
for i in hello 1 \* 2 goodbye 
do
  echo "Looping ... i is set to $i"
done

```

* * *

This is well worth trying. Make sure that you understand what is happening here. Try it without the `*` and grasp the idea, then re-read the [Wildcards](https://www.shellscript.sh/wildcards.html) section and try it again with the `*` in place. Try it also in different directories, and with the `*` surrounded by double quotes, and try it preceded by a backslash (`\*`)

In case you don't have access to a shell at the moment (it is very useful to have a shell to hand whilst reading this tutorial), the results of the above two scripts are:

```bash
Looping .... number 1
Looping .... number 2
Looping .... number 3
Looping .... number 4
Looping .... number 5

```

and, for the second example:

```bash
Looping ... i is set to hello
Looping ... i is set to 1
Looping ... i is set to (name of first file in current directory)
    ... etc ...
Looping ... i is set to (name of last file in current directory)
Looping ... i is set to 2
Looping ... i is set to goodbye

```

So, as you can see, `for` simply loops through whatever input it is given, until it runs out of input.

(adsbygoogle = window.adsbygoogle || \[\]).push({});[](https://www.shellscript.sh/loops.html)

While Loops
-----------

`while` loops can be much more fun! (depending on your idea of fun, and how often you get out of the house... )

* * *

[while.sh](https://www.shellscript.sh/eg/while.sh.txt)

```bash
#!/bin/sh
INPUT\_STRING=hello
while \[ "$INPUT\_STRING" != "bye" \]
do
  echo "Please type something in (bye to quit)"
  read INPUT\_STRING
  echo "You typed: $INPUT\_STRING"
done

```

* * *

What happens here, is that the echo and read statements will run indefinitely until you type "bye" when prompted.  
Review [Variables - Part I](https://www.shellscript.sh/variables1.html) to see why we set `INPUT_STRING=hello` before testing it. This makes it a repeat loop, not a traditional while loop.

  

The colon (`:`) always evaluates to true; whilst using this can be necessary sometimes, it is often preferable to use a real exit condition. Compare quitting the above loop with the one below; see which is the more elegant. Also think of some situations in which each one would be more useful than the other:

* * *
[while2.sh](https://www.shellscript.sh/eg/while2.sh.txt)
```bash
#!/bin/sh
while :
do
  echo "Please type something in (^C to quit)"
  read INPUT\_STRING
  echo "You typed: $INPUT\_STRING"
done

```

* * *

Another useful trick is the `while read` loop. This example uses the [case](https://www.shellscript.sh/case.html) statement, which we'll cover later. It reads from the file `myfile.txt`, and for each line, tells you what language it thinks is being used.

(note: Each line must end with a LF (newline) - if `cat myfile.txt` doesn't end with a blank line, that final line will not be processed.)

This reads the file "`myfile.txt`", one line at a time, into the variable "`$input_text`". The [case](https://www.shellscript.sh/case.html) statement then checks the value of `$input_text`. If the word that was read from `myfile.txt` was "hello" then it `echo`es the word "English". If it was "gday" then it will `echo Australian`. If the word (or words) read from a line of `myfile.txt` don't match any of the provided patterns, then the catch-all "\*" default will display the message "Unknown Language: $input\_text" - where of course "$input\_text" is the value of the line that it read in from `myfile.txt`.

* * *

[while3.sh](https://www.shellscript.sh/eg/while3.sh.txt)

```bash
#!/bin/sh
while read input\_text
do
  case $input\_text in
        hello)          echo English    ;;
        howdy)          echo American   ;;
        gday)           echo Australian ;;
        bonjour)        echo French     ;;
        "guten tag")    echo German     ;;
        \*)              echo Unknown Language: $input\_text
                ;;
   esac
done < myfile.txt

```

Let's say our `myfile.txt` file contains the following five lines:

```bash
this file is called myfile.txt and we are using it as an example input.
hello
gday
bonjour
hola

```

A sample run of this script would go like this:

```bash
$ **./while3.sh**
Unknown Language: this file is called myfile.txt and we are using it as an example input.
English
Australian
French
Unknown Language: hola

```

* * *

A handy Bash (but not Bourne Shell) tip I learned recently from the [Linux From Scratch](http://www.linuxfromscratch.org/) project is:

```bash
mkdir rc{0,1,2,3,4,5,6,S}.d
```

instead of the more cumbersome:

```bash
for runlevel in 0 1 2 3 4 5 6 S
do
  mkdir rc${runlevel}.d
done
```

And this can be done recursively, too:

```bash
$ cd /
$ ls -ld {,usr,usr/local}/{bin,sbin,lib}
drwxr-xr-x    2 root     root         4096 Oct 26 01:00 /bin
drwxr-xr-x    6 root     root         4096 Jan 16 17:09 /lib
drwxr-xr-x    2 root     root         4096 Oct 27 00:02 /sbin
drwxr-xr-x    2 root     root        40960 Jan 16 19:35 usr/bin
drwxr-xr-x   83 root     root        49152 Jan 16 17:23 usr/lib
drwxr-xr-x    2 root     root         4096 Jan 16 22:22 usr/local/bin
drwxr-xr-x    3 root     root         4096 Jan 16 19:17 usr/local/lib
drwxr-xr-x    2 root     root         4096 Dec 28 00:44 usr/local/sbin
drwxr-xr-x    2 root     root         8192 Dec 27 02:10 usr/sbin
```

* * *

Copyright © 2000 - 2026 Steve Parker


本文转自 [https://www.shellscript.sh/loops.html](https://www.shellscript.sh/loops.html)，如有侵权，请联系删除。