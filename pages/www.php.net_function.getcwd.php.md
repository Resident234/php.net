# getcwd




<div class="phpcode"><span class="html">
getcwd() returns the path of the &quot;main&quot; script referenced in the URL.<br><br>dirname(__FILE__) will return the path of the script currently executing.<br><br>I had written a script that required several class definition scripts from the same directory. It retrieved them based on filename matches and used getcwd to figure out where they were.<br><br>Didn&apos;t work so well when I needed to call that first script from a new file in a different directory.</span>
</div>
  

#


<div class="phpcode"><span class="html">
It appears there is a change in functionality in PHP5 from PHP4 when using the CLI tool.&#xA0; Here is the example: -<br><br>cd /tmp<br><br>cat &gt; foo.php &lt;&lt; END<br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">print </span><span class="default">getcwd</span><span class="keyword">() . </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span>END<br><br>cd /<br><br>php -q /tmp/foo.php<br><br>PHP4 returns /tmp<br>PHP5 returns /<br><br>Something to be aware of.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.getcwd.php)

**[To root](/README.md)**