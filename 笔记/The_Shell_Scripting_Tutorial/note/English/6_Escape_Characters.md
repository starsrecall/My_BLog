# Escape Characters

Certain characters are significant to the shell; we have seen, for example, that the use of double quotes (`"`) characters affect how spaces and TAB characters are treated, for example:

```bash
$ echo Hello       World
Hello World
$ echo "Hello       World"
Hello     World

```

So how do we display: `Hello    "World"` ?

```bash
$ echo "Hello   \"World\""

```

The first and last " characters wrap the whole lot into one parameter passed to `echo` so that the spacing between the two words is kept as is. But the code:

```bash
$ echo "Hello   " World ""

```

would be interpreted as three parameters:

1.  "Hello   "
2.  World
3.  ""

So the output would be

```bash
Hello    World

```

Note that we lose the quotes entirely. This is because the first and second quotes mark off the Hello and following spaces; the second argument is an unquoted "World" and the third argument is the empty string; "".

Notice that this:

```bash
$ echo "Hello   "World""

```

is actually only one parameter (no spaces between the quoted parameters), and that you can test this by replacing the `echo` command with (for example) `ls`.

Most characters (`*`, `'`, etc) are not interpreted (ie, they are taken literally) by means of placing them in double quotes (""). They are taken as is and passed on to the command being called. An example using the asterisk (`*`) goes:

```bash
$ echo *
case.shtml escape.shtml first.shtml 
functions.shtml hints.shtml index.shtml 
ip-primer.txt raid1+0.txt
$ echo *txt
ip-primer.txt raid1+0.txt
$ echo "*"
*
$ echo "*txt"
*txt

```

In the first example, `*` is expanded to mean all files in the current directory.
In the second example, `*txt` means all files ending in `txt`.
In the third, we put the `*` in double quotes, and it is interpreted literally.
In the fourth example, the same applies, but we have appended `txt` to the string.

However, `"`, `$`, `` ` ``, and `\` are still interpreted by the shell, even when they're in double quotes.
The backslash (`\`) character is used to mark these special characters so that they are not interpreted by the shell, but passed on to the command being run (for example, `echo`).
So to output the string: (Assuming that the value of `$X` is 5):

```bash
A quote is ", backslash is \, backtick is `.
A few spaces are    and dollar is $. $X is 5.
```

we would have to write:

```bash
$ echo "A quote is \", backslash is \\, backtick is \`."
A quote is ", backslash is \, backtick is `.
$ echo "A few spaces are    and dollar is \$. \$X is ${X}."
A few spaces are    and dollar is $. $X is 5.

```

We have seen why the `"` is special for preserving spacing. Dollar (`$`) is special because it marks a variable, so `$X` is replaced by the shell with the contents of the variable `X`. Backslash (`\`) is special because it is itself used to mark other characters off; we need the following options for a complete shell:

```bash
$ echo "This is \\ a backslash"
This is \ a backslash
$ echo "This is \" a quote and this is \\ a backslash"
This is " a quote and this is \ a backslash

```

So backslash itself must be escaped to show that it is to be taken literally. The other special character, the backtick, is discussed later in Chapter 12, [External Programs](https://www.shellscript.sh/external.html).

---

Copyright © 2000 - 2026 Steve Parker

本文转自 [https://www.shellscript.sh/escape.html](https://www.shellscript.sh/escape.html)，如有侵权，请联系删除。