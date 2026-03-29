# Philosophy

Shell script programming has a bit of a bad press amongst some Unix systems administrators. This is normally because of one of two things:

*   The speed at which an interpreted program will run as compared to a C program, or even an interpreted Perl program.
*   Since it is easy to write a simple batch-job type shell script, there are a lot of poor quality shell scripts around.

It is partly due to this that there is a certain machismo associated with creating _good_ shell scripts. Scripts which can be used as CGI programs, for example, without losing out too much in speed to Perl (though both would lose to C, in many cases, were speed the only criterion).  
There are a number of factors which can go into good, clean, quick, shell scripts.

*   The most important criteria must be a clear, readable layout.
*   Second is avoiding unnecessary commands.

A clear layout makes the difference between a shell script appearing as "black magic" and one which is easily maintained and understood.  
You may be forgiven for thinking that with a simple script, this is not too significant a problem, but two things here are worth bearing in mind.

1.  First, a simple script will, more often than anticipated, grow into a large, complex one.
2.  Secondly, if nobody else can understand how it works, you will be lumbered with maintaining it yourself for the rest of your life!

Something about shell scripts seems to make them particularly likely to be badly indented, and since the main control structures are if/then/else and loops, indentation is critical for understanding what a script does.

One weakness in many shell scripts is lines such as:

```bash
cat /tmp/myfile | grep "mystring"
```

which would run much faster as:

```bash
grep "mystring" /tmp/myfile
```

Not much, you may consider; the OS has to load up the `/bin/grep` executable, which is a reasonably small 75600 bytes on my system, open a `pipe` in memory for the transfer, load and run the `/bin/cat` executable, which is an even smaller 9528 bytes on my system, attach it to the input of the pipe, and let it run.

Of course, this kind of thing is what the OS is there for, and it's normally pretty efficient at doing it. But if this command were in a loop being run many times over, the saving of not locating and loading the `cat` executable, setting up and releasing the pipe, can make some difference, especially in, say, a CGI environment where there are enough other factors to slow things down without the script itself being too much of a hurdle. Some Unices are more efficient than others at what they call "building up and tearing down processes" - i.e., loading them up, executing them, and clearing them away again. But however good your flavour of Unix is at doing this, it'd rather not have to do it at all.

As a result of this, you may hear mention of the Useless Use of Cat Award (UUoC), also known in some circles as **The Award For The Most Gratuitous Use Of The Word Cat In A Serious Shell Script** being bandied about on the `comp.unix.shell` newsgroup from time to time. This is purely a way of peers keeping each other in check, and making sure that things are done right.

Which leads me nicely on to something else: Don't _ever_ feel too close to your own shell scripts; by their nature, the source cannot be closed. If you supply a customer with a shell script, s/he can inspect it quite easily. So you might as well accept that it will be inspected by anyone you pass it to; use this to your advantage with the [GPL](http://www.gnu.org/copyleft/gpl.html) - encourage people to give you feedback and bugfixes for free!



---



Copyright © 2000 - 2026 Steve Parker

本文转自 [https://www.shellscript.sh/philosophy.html](https://www.shellscript.sh/philosophy.html)，如有侵权，请联系删除。