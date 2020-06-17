# sscanf




<div class="phpcode"><span class="html">
this function is a great way to get integer rgb values from the html equivalent hex.<br><br>list($r, $g, $b) = sscanf(&apos;00ccff&apos;, &apos;%2x%2x%2x&apos;);</span>
</div>
  

#


<div class="phpcode"><span class="html">
FYI - if you are trying to scan from a string which contains a filename with extension. For instance:<br><br><span class="default">&lt;?php<br><br>$out </span><span class="keyword">= </span><span class="default">sscanf</span><span class="keyword">(</span><span class="string">&apos;file_name.gif&apos;</span><span class="keyword">, </span><span class="string">&apos;file_%s.%s&apos;</span><span class="keyword">, </span><span class="default">$fpart1</span><span class="keyword">, </span><span class="default">$fpart2</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>The scanned string in the $fpart1 parameter turns out to be &apos;name.gif&apos; and $fpart2 will be NULL.<br><br>To get around this you can simply replace the &quot;.&quot; with a space or another &quot;white-space like&quot; string sequence.<br><br>I didn&apos;t see any other comments on regarding string literals which contain a &apos;.&apos; so I thought I&apos;d mention it. The subtle characteristics of having &quot;white-space delimited&quot; content I think can be a source of usage contention. Obviously, another way to go is regular expressions in this instance, but for newer users this may be helpful.<br><br>Just in case someone else spent 10 minutes of frustration like I did. This was seen on PHP Version 5.2.3-1ubuntu6.3.<br><br>Searching the bug reports shows another users misunderstanding: <a href="http://bugs.php.net/bug.php?id=7793" rel="nofollow" target="_blank">http://bugs.php.net/bug.php?id=7793</a></span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.sscanf.php)

**[To root](/README.md)**