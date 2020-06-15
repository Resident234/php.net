# popen




<div class="phpcode"><span class="html">
If you try to execute a command under Windows the PHP script normally waits until the process has been terminated. Executing long-term processes pauses a PHP script even if you don&apos;t want to wait for the end of the process.
<br>
<br>It wasn&apos;t easy to find this beautiful example how to start a process under Windows without waiting for its termination:
<br>
<br><span class="default">&lt;?php
<br>$commandString </span><span class="keyword">= </span><span class="string">&apos;start /b c:\\programToRun.exe -attachment &quot;c:\\temp\file1.txt&quot;&apos;</span><span class="keyword">;
<br></span><span class="default">pclose</span><span class="keyword">(</span><span class="default">popen</span><span class="keyword">(</span><span class="default">$commandString</span><span class="keyword">, </span><span class="string">&apos;r&apos;</span><span class="keyword">));
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If, on windows, you need to start a batch file that needs administrator privileges, then you can make a shortcut to the batch file, click properties, check to on &quot;run as administrator&quot; on one of the property pages, and then double-click the shortcut once (to initialize that &quot;run as administrator&quot; business).<br><br>using popen(&quot;/path/to/shortcut.lnk&quot;) will then run your batch file with administrator privileges.<br><br>handy for when you want to use cli php to do some long running tasks and that php-cli needs to use sessions..</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.popen.php)

**[To root](/README.md)**