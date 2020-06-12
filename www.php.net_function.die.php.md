# die




<div class="phpcode"><span class="html">
It is poor design to rely on die() for error handling in a web site because it results in an ugly experience for site users: a broken page and - if they&apos;re lucky - an error message that is no help to them at all. As far as they are concerned, when the page breaks, the whole site might as well be broken.<br><br>If you ever want the public to use your site, always design it to handle errors in a way that will allow them to continue using it if possible. If it&apos;s not possible and the site really is broken, make sure that you find out so that you can fix it. die() by itself won&apos;t do either.<br><br>If a supermarket freezer breaks down, a customer who wanted to buy a tub of ice cream doesn&apos;t expect to be kicked out of the building.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Beware that when using PHP on the command line, die(&quot;Error&quot;) simply prints &quot;Error&quot; to STDOUT and terminates the program with a normal exit code of 0.<br><br>If you are looking to follow UNIX conventions for CLI programs, you may consider the following:<br><br><span class="default">&lt;?php<br>fwrite</span><span class="keyword">(</span><span class="default">STDERR</span><span class="keyword">, </span><span class="string">&quot;An error occurred.\n&quot;</span><span class="keyword">);<br>exit(</span><span class="default">1</span><span class="keyword">); </span><span class="comment">// A response code other than 0 is a failure<br></span><span class="default">?&gt;<br></span><br>In this way, when you pipe STDOUT to a file, you may see error messages in the console and BASH scripts can test for a response code of 0 for success:<br><br>rc@adl-dev-01:~$ php die.php &gt; test<br>An error occurred.<br>rc@adl-dev-01:~$ echo $?<br>1<br><br>Ideally, PHP would write all Warnings, Fatal Errors, etc on STDERR, but that&apos;s another story.</span>
</div>
  

#


<div class="phpcode"><span class="html">
die doesn&apos;t prevent destructors from being run, so the script doesn&apos;t exit immediately, it still goes through cleanup routines.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.die.php)

**[⬆ to root](/)**