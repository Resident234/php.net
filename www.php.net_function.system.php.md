# system




<div class="phpcode"><span class="html">
This is for WINDOWS users. I am running apache and I have been trying for hours now to capture the output of a command. <br><br>I&apos;d tried everything that is written here and then continued searching online with no luck at all. The output of the command was never captured. All I got was an empty array.<br><br>Finally, I found a comment in a blog by a certain amazing guy that solved my problems. <br><br>Adding the string &apos; 2&gt;&amp;1&apos; to the command name finally returned the output!! This works in exec() as well as system() in PHP since it uses stream redirection to redirect the output to the correct place!<br><br>system(&quot;yourCommandName 2&gt;&amp;1&quot;,$output) ;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.system.php)

**[To root](/README.md)**