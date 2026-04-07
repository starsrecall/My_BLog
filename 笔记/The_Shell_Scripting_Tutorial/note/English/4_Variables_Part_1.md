Variables - Part 1
------------------

​	Just about every programming language in existence has the concept of _variables_ - a symbolic name for a chunk of memory to which we can assign values, read and manipulate its contents. The Bourne shell is no exception, and this section introduces that idea. This is taken further in [Variables - Part II](https://www.shellscript.sh/variables2.html) which looks into variables which are set for us by the environment.  
​	Let's look back at our first Hello World example. This could be done using variables (though it's such a simple example that it doesn't really warrant it!)  
​	Note that there must be no spaces around the "`=`" sign: `VAR=value` works; `VAR = value` doesn't work. In the first case, the shell sees the "`=`" symbol and treats the command as a variable assignment. In the second case, the shell assumes that VAR must be the name of a command and tries to execute it.  
​	If you think about it, this makes sense - how else could you tell it to run the command VAR with its first argument being "=" and its second argument being "value"?  
Enter the following code into var.sh:

[var.sh](https://www.shellscript.sh/eg/var.sh.txt)
```bash
#!/bin/sh  
MY\_MESSAGE\="Hello World"  
echo $MY\_MESSAGE
```
This assigns the string "Hello World" to the variable `MY_MESSAGE` then `echo`es out the value of the variable.  
	Note that we need the quotes around the string Hello World. Whereas we could get away with `echo Hello World` because echo will take any number of parameters, a variable can only hold one value, so a string with spaces must be quoted so that the shell knows to treat it all as one. Otherwise, the shell will try to execute the command `World` after assigning `MY_MESSAGE=Hello`

​	The shell does not care about types of variables; they may store strings, integers, real numbers - anything you like.  
People used to Perl may be quite happy with this; if you've grown up with C, Pascal, or worse yet Ada, this may seem quite strange. 
In truth, these are all stored as strings, but routines which expect a number can treat them as such. 
​	If you assign a string to a variable then try to add 1 to it, you will not get away with it:

```bash
$ x="hello"  
$ expr $x + 1  
expr: non-numeric argument  
$
```

​	This is because the external program `expr` only expects numbers. But there is no syntactic difference between:

```bash
MY_MESSAGE="Hello World"  
MY_SHORT_MESSAGE=hi  
MY_NUMBER=1  
MY_PI=3.142  
MY_OTHER_PI="3.142"  
MY_MIXED=123abc
```

​	Note though that special characters must be properly escaped to avoid interpretation by the shell.  
​	This is discussed further in Chapter 6, [Escape Characters](https://www.shellscript.sh/escape.html).

​	We can interactively set variable names using the `read` command; the following script asks you for your name then greets you personally:

[var2.sh](https://www.shellscript.sh/eg/var2.sh.txt)

```bash
#!/bin/sh  
echo What is your name?  
read MY\_NAMEecho "Hello $MY\_NAME - hope you're well."
echo "Hello $MY_NAME - hope you're well."
```
​	In an early draft I had missed out the double-quotes in the final line, which meant that the single-quote in the word "you're" was unmatched, causing an error. It is this kind of thing which can drive a shell programmer crazy, so watch out for them!

​	This is using the shell-builtin command `read` which reads a line from standard input into the variable supplied. 
​	Note that even if you give it your full name and don't use double quotes around the `echo` command, it still outputs correctly. How is this done? With the `MY_MESSAGE` variable earlier we had to put double quotes around it to set it.  
​	What happens, is that the `read` command automatically places quotes around its input, so that spaces are treated correctly. (You will need to quote the output, of course - e.g. `echo "$MY_MESSAGE"`).

[](https://www.shellscript.sh/variables1.html)

Scope of Variables
------------------

​	Variables in the Bourne shell do not have to be declared, as they do in languages like C. But if you try to read an undeclared variable, the result is the empty string. You get no warnings or errors. This can cause some subtle bugs - if you assign  
`MY_OBFUSCATED_VARIABLE=Hello`  
and then  
`echo $MY_OSFUCATED_VARIABLE`  
Then you will get nothing (as the second OBFUSCATED is mis-spelled).

​	There is a command called `export` which has a fundamental effect on the scope of variables. In order to really know what's going on with your variables, you will need to understand something about how this is used.

Create a small shell script, `myvar2.sh`:

[myvar2.sh](https://www.shellscript.sh/eg/myvar2.sh.txt)

```bash
#!/bin/sh  
echo "MYVAR is: $MYVAR"  
MYVAR\="hi there"  
echo "MYVAR is: $MYVAR"

```

Now run the script:

```bash
$ ./myvar2.sh  
MYVAR is:  
MYVAR is: hi there
```

MYVAR hasn't been set to any value, so it's blank. Then we give it a value, and it has the expected result. 
Now run:

```bash
$ MYVAR\=hello  
$ ./myvar2.sh  
MYVAR is:  
MYVAR is: hi there
```

​	It's still not been set! What's going on?!
​	When you call `myvar2.sh` from your interactive shell, a new shell is spawned to run the script. This is partly because of the `#!/bin/sh` line at the start of the script, which we discussed [earlier](https://www.shellscript.sh/first.html).  
​	We need to `export` the variable for it to be inherited by another program - including a shell script. Type:

```bash
$ export MYVAR  
$ ./myvar2.sh  
MYVAR is: hello  
MYVAR is: hi there
```

​	Now look at line 3 of the script: this is changing the value of `MYVAR`. But there is no way that this will be passed back to your interactive shell. Try reading the value of `MYVAR`:

```
$ echo $MYVAR  
hello  
$
```

​	Once the shell script exits, its environment is destroyed. But `MYVAR` keeps its value of `hello` within your interactive shell.  
In order to receive environment changes back from the script, we must _source_ the script - this effectively runs the script within our own interactive shell, instead of spawning another shell to run it.  
​	We can source a script via the "." (dot) command:

```
$ MYVAR\=hello  
$ echo $MYVAR  
hello  
$ . ./myvar2.sh  
MYVAR is: hello  
MYVAR is: hi there  
$ echo $MYVAR  
hi there
```

​	The change has now made it out into our shell again! This is how your `.profile` or `.bash_profile` file works, for example.  
Note that in this case, we don't need to `export MYVAR`.  
​	An easy mistake to make is to say `echo MYVAR` instead of `echo $MYVAR` - unlike most languages, the dollar (`$`) symbol is required when getting the value of a variable, but must not be used when setting the value of the variable. An easy mistake to make when starting out in shell scripting.  
​	One other thing worth mentioning at this point about variables, is to consider the following shell script:

```
#!/bin/sh  
echo "What is your name?"  
read USER\_NAMEecho "Hello $USER\_NAME"  
echo "I will create you a file called $USER\_NAME\_file"  
touch $USER\_NAME\_file
```

​	Think about what result you would expect. For example, if you enter "steve" as your USER\_NAME, should the script create `steve_file`?  
​	Actually, no. This will cause an error unless there is a variable called `USER_NAME_file`. The shell does not know where the variable ends and the rest starts. How can we define this?  
The answer is, that we enclose the variable itself in _curly brackets_:

[user.sh](https://www.shellscript.sh/eg/user.sh.txt)

```bash
#!/bin/sh  
echo "What is your name?"  
read USER_NAMEecho "Hello $USER_NAME"  
echo "I will create you a file called ${USER_NAME}_file"  
touch "${USER_NAME}_file"
```

​	The shell now knows that we are referring to the variable `USER_NAME` and that we want it suffixed with "`_file`". This can be the downfall of many a new shell script programmer, as the source of the problem can be difficult to track down.

​	Also note the quotes around `"${USER_NAME}_file"` - if the user entered "Steve Parker" (note the space) then without the quotes, the arguments passed to `touch` would be `Steve` and `Parker_file` - that is, we'd effectively be saying `touch Steve Parker_file`, which is two files to be `touch`ed, not one. The quotes avoid this. Thanks to Chris for highlighting this.

  [Previous: A First Script](https://www.shellscript.sh/first.html)  [Next: Wildcards](https://www.shellscript.sh/wildcards.html)   


Copyright © 2000 - 2026 Steve Parker

  

本文转自 [https://www.shellscript.sh/variables1.html](https://www.shellscript.sh/variables1.html)，如有侵权，请联系删除。