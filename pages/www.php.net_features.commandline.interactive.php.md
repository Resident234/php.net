# Interactive shell



Interactive Shell and Interactive Mode are not the same thing, despite the similar names and functionality.<br><br>If you type &apos;php -a&apos; and get a response of &apos;Interactive Shell&apos; followed by a &apos;php&gt;&apos; prompt, you have interactive shell available (PHP was compiled with readline support). If instead you get a response of &apos;Interactive mode enabled&apos;, you DO NOT have interactive shell available and this article does not apply to you.<br><br>You can also check &apos;php -m&apos; and see if readline is listed in the output - if not, you don&apos;t have interactive shell.<br><br>Interactive mode is essentially like running php with stdin as the file input. You just type code, and when you&apos;re done (Ctrl-D), php executes whatever you typed as if it were a normal PHP (PHTML) file - hence you start in interactive mode with &apos;

```
<?php' in order to execute code.

Interactive shell evaluates every expression as you complete it (with ; or }), reports errors without terminating execution, and supports standard shell functionality via readline (history, tab completion, etc). It's an enhanced version of interactive mode that is ONLY available if you have the required libraries, and is an actual PHP shell that interprets everything you type as PHP code - using '

```
<?php' will cause a parse error.

Finally, if you're running on Windows, you're probably screwed. From what I'm seeing in other comments here, you don't have readline, and without readline there is no interactive shell.?>
```
  

#

In Windows, press Enter after your ending PHP tag and then hit Ctrl-Z to denote the end-of-file:<br><br>C:\&gt;php -a<br>Interactive mode enabled<br><br>

```
<?php
echo "Hello, world!";
?>
```
<br>^Z<br>Hello, world!<br><br>You can use the up and down arrows in interactive mode to recall previous code you ran.  

#

It seems the interactive shell cannot be made to work in WIN environments at the moment.  <br><br>Using "php://stdin", it shouldn&apos;t be too difficult to roll your own.  You can partially mimic the shell by calling this simple script (Note: Window&apos;s cmd already has an input history calling feature using the up/down keys, and that functionality will still be available during execution here):<br><br>

```
<?php

$fp = fopen("php://stdin", "r");
$in = '';
while($in != "quit") {
    echo "php&gt; ";
    $in=trim(fgets($fp));
    eval ($in);
    echo "\n";
    }
    
?>
```
<br><br>Replace &apos;eval&apos; with code to parse the input string, validate it using is_callable and other variable handling functions, catch fatal errors before they happen, allow line-by-line function defining, etc.  Though Readline is not available in Windows, for more tips and examples for workarounds, see http://www.php.net/manual/en/ref.readline.php  

#

For use interactive mode enabled on GNU/Linux on distros Debian/Ubuntu/LinuxMint you must install "php*-cli" and "php*-readline" packages from official repository.<br>Example:<br> &gt;$sudo aptitude install php5-cli php5-readline<br><br>After that you can use interactive mode.<br>Example:<br>~ $ php -a<br>Interactive mode enabled<br><br>php &gt;echo "hola mundo!\n";<br>hola mundo!<br>php &gt;<br><br>I hope somebody help it!  

#

Just a few more notes to add...<br><br>1) Hitting return does literally mean "execute this command".  Semicolon to note end of line is still required.  Meaning, doing the following will produce a parse error:<br><br>php &gt; print "test"<br>php &gt; print "asdf";<br><br>Whereas doing the following is just fine:<br><br>php &gt; print "test"<br>php &gt; ."asdf";<br><br>2) Fatal errors may eject you from the shell:<br><br>name@local:~$ php -a<br>php &gt; asdf();<br><br>Fatal Error: call to undefined function...<br>name@local:~$<br><br>3) User defined functions are not saved in history from shell session to shell session.<br><br>4) Should be obvious, but to quit the shell, just type "quit" at the php prompt.<br><br>5) In a sense, the shell interaction can be thought of as linearly following a regular php file, except it&apos;s live and dynamic.  If you define a function that you&apos;ve already defined earlier in your current shell, you will receive a fatal "function already defined" error only upon entering that closing bracket.  And, although "including" a toolset of custom functions or a couple of script addon php files is rather handy, should you edit those files and wish to "reinclude" it again, you&apos;ll cause a fatal "function x already defined" error.  

#

[Official documentation page](https://www.php.net/manual/en/features.commandline.interactive.php)

**[To root](/README.md)**