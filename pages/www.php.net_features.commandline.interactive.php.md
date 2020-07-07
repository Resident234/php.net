# Interactive shell





Interactive Shell and Interactive Mode are not the same thing, despite the similar names and functionality.

If you type &apos;php -a&apos; and get a response of &apos;Interactive Shell&apos; followed by a &apos;php&gt;&apos; prompt, you have interactive shell available (PHP was compiled with readline support). If instead you get a response of &apos;Interactive mode enabled&apos;, you DO NOT have interactive shell available and this article does not apply to you.

You can also check &apos;php -m&apos; and see if readline is listed in the output - if not, you don&apos;t have interactive shell.

Interactive mode is essentially like running php with stdin as the file input. You just type code, and when you&apos;re done (Ctrl-D), php executes whatever you typed as if it were a normal PHP (PHTML) file - hence you start in interactive mode with &apos;

```
<?php&apos; in order to execute code.

Interactive shell evaluates every expression as you complete it (with ; or }), reports errors without terminating execution, and supports standard shell functionality via readline (history, tab completion, etc). It&apos;s an enhanced version of interactive mode that is ONLY available if you have the required libraries, and is an actual PHP shell that interprets everything you type as PHP code - using &apos;

```
<?php&apos; will cause a parse error.

Finally, if you&apos;re running on Windows, you&apos;re probably screwed. From what I&apos;m seeing in other comments here, you don&apos;t have readline, and without readline there is no interactive shell.

  

#



In Windows, press Enter after your ending PHP tag and then hit Ctrl-Z to denote the end-of-file:



C:\&gt;php -a

Interactive mode enabled





```
<?php

echo &quot;Hello, world!&quot;;

?>
```


^Z

Hello, world!



You can use the up and down arrows in interactive mode to recall previous code you ran.

  

#



It seems the interactive shell cannot be made to work in WIN environments at the moment.&#xA0; 

Using &quot;php://stdin&quot;, it shouldn&apos;t be too difficult to roll your own.&#xA0; You can partially mimic the shell by calling this simple script (Note: Window&apos;s cmd already has an input history calling feature using the up/down keys, and that functionality will still be available during execution here):



```
<?php

$fp = fopen(&quot;php://stdin&quot;, &quot;r&quot;);
$in = &apos;&apos;;
while($in != &quot;quit&quot;) {
&#xA0; &#xA0; echo &quot;php&gt; &quot;;
&#xA0; &#xA0; $in=trim(fgets($fp));
&#xA0; &#xA0; eval ($in);
&#xA0; &#xA0; echo &quot;\n&quot;;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
?>
```


Replace &apos;eval&apos; with code to parse the input string, validate it using is_callable and other variable handling functions, catch fatal errors before they happen, allow line-by-line function defining, etc.&#xA0; Though Readline is not available in Windows, for more tips and examples for workarounds, see http://www.php.net/manual/en/ref.readline.php

  

#



For use interactive mode enabled on GNU/Linux on distros Debian/Ubuntu/LinuxMint you must install &quot;php*-cli&quot; and &quot;php*-readline&quot; packages from official repository.
Example:
 &gt;$sudo aptitude install php5-cli php5-readline

After that you can use interactive mode.
Example:
~ $ php -a
Interactive mode enabled

php &gt;echo &quot;hola mundo!\n&quot;;
hola mundo!
php &gt;

I hope somebody help it!

  

#



Just a few more notes to add...

1) Hitting return does literally mean &quot;execute this command&quot;.&#xA0; Semicolon to note end of line is still required.&#xA0; Meaning, doing the following will produce a parse error:

php &gt; print &quot;test&quot;
php &gt; print &quot;asdf&quot;;

Whereas doing the following is just fine:

php &gt; print &quot;test&quot;
php &gt; .&quot;asdf&quot;;

2) Fatal errors may eject you from the shell:

name@local:~$ php -a
php &gt; asdf();

Fatal Error: call to undefined function...
name@local:~$

3) User defined functions are not saved in history from shell session to shell session.

4) Should be obvious, but to quit the shell, just type &quot;quit&quot; at the php prompt.

5) In a sense, the shell interaction can be thought of as linearly following a regular php file, except it&apos;s live and dynamic.&#xA0; If you define a function that you&apos;ve already defined earlier in your current shell, you will receive a fatal &quot;function already defined&quot; error only upon entering that closing bracket.&#xA0; And, although &quot;including&quot; a toolset of custom functions or a couple of script addon php files is rather handy, should you edit those files and wish to &quot;reinclude&quot; it again, you&apos;ll cause a fatal &quot;function x already defined&quot; error.

  

#

[Official documentation page](https://www.php.net/manual/en/features.commandline.interactive.php)

**[To root](/README.md)**