# require




<div class="phpcode"><span class="html">
Remember, when using require that it is a statement, not a function. It&apos;s not necessary to write:<br><span class="default">&lt;?php<br> </span><span class="keyword">require(</span><span class="string">&apos;somefile.php&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>The following:<br><span class="default">&lt;?php<br></span><span class="keyword">require </span><span class="string">&apos;somefile.php&apos;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>Is preferred, it will prevent your peers from giving you a hard time and a trivial conversation about what require really is.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.require.php)

**[To root](/)**