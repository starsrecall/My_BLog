# The Shell Scripting Tutorial

*   [Home](https://www.shellscript.sh/#home)
*   [Philosophy](https://www.shellscript.sh/philosophy.html)
*   [A First Script](https://www.shellscript.sh/first.html)
*   [Variables (Part 1)](https://www.shellscript.sh/variables1.html)
*   [Wildcards](https://www.shellscript.sh/wildcards.html)
*   [Escape Characters](https://www.shellscript.sh/escape.html)
*   [Loops](https://www.shellscript.sh/loops.html)
*   [Test](https://www.shellscript.sh/test.html)
*   [Case](https://www.shellscript.sh/case.html)
*   [Variables (Part 2)](https://www.shellscript.sh/variables2.html)
*   [Variables (Part 3)](https://www.shellscript.sh/variables3.html)
*   [External Programs](https://www.shellscript.sh/external.html)
*   [Functions](https://www.shellscript.sh/functions.html)
*   [Hints and Tips](https://www.shellscript.sh/hints.html)
*   [Quick Reference](https://www.shellscript.sh/quickref.html)
*   [Interactive Shell](https://www.shellscript.sh/interactive.html)
*   [Exercises](https://www.shellscript.sh/exercises.html)
*   [Tips and Examples](https://www.shellscript.sh/examples/)
*   [Shell Scripting: Expert Recipes](https://amzn.to/3J1JYSx)

----


Purpose Of This Tutorial
------------------------

This tutorial is written to help people understand some of the basics of **shell script** programming (aka **shell scripting**), and hopefully to introduce some of the possibilities of simple but powerful programming available under the Bourne shell. As such, it has been written as a basis for one-on-one or group tutorials and exercises, and as a reference for subsequent use.

Getting The Most Recent Version Of This Tutorial
------------------------------------------------

You are reading Version 4.5b, last updated 6th June 2023.

The most recent version of this tutorial is always available at: [https://www.shellscript.sh](https://www.shellscript.sh/). Always check there for the latest copy. (If you are reading this at some different address, it is probably a copy of the real site, and therefore may be out of date).

A Brief History of sh
---------------------

Steve Bourne wrote the original Bourne shell which appeared in the Seventh Edition Bell Labs Research version of Unix. Many variants have come and gone over time (csh, ksh, and so on).

This tutorial restricts itself to being Bourne shell compatible, to provide a baseline. This tutorial covers all shell scripting basics. The [Shell Scripting Examples](https://www.shellscript.sh/examples/) section of the tutorial adds additional examples in particular of how the Bash shell provides additional useful functionality over the standard Bourne shell.

Audience
--------

This tutorial assumes some prior experience; namely:

*   **Use of** an **interactive** Unix/Linux shell
*   **Minimal programming knowledge** - use of variables, functions, is useful background knowledge
*   Understanding of **some** Unix/Linux commands, and **competence** in using **some** of the **more common** ones. (_ls_, _cp_, _echo_, etc)
*   Programmers of **ruby**, **perl**, **python**, **C**, **Pascal**, or any programming language (even BASIC) who can maybe read shell scripts, but don't feel they understand exactly how they work.

You may want to review some of the [feedback that this tutorial has received](https://www.shellscript.sh/feedback.html) to see how useful you might find it.

Typographical Conventions Used in This Tutorial
-----------------------------------------------

Code segments and script output will be displayed as monospaced text. Command-line entries will be preceded by the Dollar sign ($). If your prompt is different, enter the command:

```
PS1="$ " ; export PS1
```

Then your interactions should match the examples given (such as `./my-script.sh` below). Script output (such as "Hello World" below) is displayed at the start of the line.

```
$ echo '#!/bin/sh' > my-script.sh
$ echo 'echo Hello World' >> my-script.sh
$ chmod 755 my-script.sh
$ ./my-script.sh
Hello World
$
```
Entire scripts will be shown with a like this, and include a reference to the plain text of the script, where available like this: [my-script.sh](https://www.shellscript.sh/eg/my-script.sh.txt)

```shell
#!/bin/sh
# This is a comment!
echo Hello World        # This is a comment, too!
```

Note that to make a file executable, you must set the e**X**ecutable bit, and for a shell script, the **R**eadable bit must also be set. So it is likely that you will need to change the permissions on your script, to make it executable. If your script is named "myscript.sh" then you will need to change its permissions, like this:
```bash
$ chmod u+rx myscript.sh
$ ./myscript.sh
```

[Get this tutorial as a PDF for only $5](https://gum.co/shellscript)

* * *

 [Next: Philosophy](https://www.shellscript.sh/philosophy.html)   

* * *

My Paperbacks and eBooks
------------------------

My Shell Scripting books, available in Paperback and eBook formats. This [tutorial](http://amzn.to/2mPj2tK) is more of a general introduction to Shell Scripting, the longer [Shell Scripting: Expert Recipes for Linux, Bash and more](http://amzn.to/2mPhTlK) book covers every aspect of Bash in detail.

<table class="booklist"><tbody><tr><td><a target="_blank" href="http://amzn.to/2mPj2tK"><center><img src="https://www.shellscript.sh/amzn/tutorial.jpg" width="125px" height="200px" alt="Shell Scripting Tutorial"></center><br>Shell Scripting Tutorial</a> is this tutorial, in 88-page Paperback and eBook formats. Convenient to read on the go, and in paperback format good to keep by your desk as an ever-present companion.<br><br>Also available in PDF form from Gumroad:<a class="gumroad-button" href="https://gum.co/shellscript" target="_blank">Get this tutorial as a PDF</a></td><td><a target="_blank" href="http://amzn.to/2mPhTlK"><center><img src="https://www.shellscript.sh/amzn/shellscripting.jpg" width="159px" height="200px" alt="Shell Scripting: Expert Recipes for Linux, Bash and more"></center><br>Shell Scripting: Expert Recipes for Linux, Bash and more</a> is my 564-page book on Shell Scripting. The first half covers all of the features of the shell in every detail; the second half has real-world shell scripts, organised by topic, along with detailed discussion of each script.</td></tr></tbody></table>

Copyright © 2000 - 2026 Steve Parker

本文转自 [https://www.shellscript.sh/](https://www.shellscript.sh/)，如有侵权，请联系删除。