# chmod




<div class="phpcode"><span class="html">
BEWARE, a couple of the examples in the comments suggest doing something like this:<br><br>chmod(file_or_dir_name, intval($mode, 8));<br><br>However, if $mode is an integer then intval( ) won&apos;t modify it.&#xA0; So, this code...<br><br>$mode = 644;<br>chmod(&apos;/tmp/test&apos;, intval($mode, 8));<br><br>...produces permissions that look like this:<br><br>1--w----r-T<br><br>Instead, use octdec( ), like this:<br><br>chmod(file_or_dir_name, octdec($mode));<br><br>See also: <a href="http://www.php.net/manual/en/function.octdec.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/function.octdec.php</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
BEWARE using quotes around the second parameter...<br><br>If you use quotes eg<br><br>chmod (file, &quot;0644&quot;);<br><br>php will not complain but will do an implicit conversion to an int before running chmod. Unfortunately the implicit conversion doesn&apos;t take into account the octal string so you end up with an integer version 644, which is 1204 octal</span>
</div>
  

#


<div class="phpcode"><span class="html">
Usefull reference:<br><br>Value&#xA0; &#xA0; Permission Level<br>400&#xA0; &#xA0; Owner Read<br>200&#xA0; &#xA0; Owner Write<br>100&#xA0; &#xA0; Owner Execute<br>40&#xA0; &#xA0; Group Read<br>20&#xA0; &#xA0; Group Write<br>10&#xA0; &#xA0; Group Execute<br>4&#xA0; &#xA0; Global Read<br>2&#xA0; &#xA0; Global Write<br>1&#xA0; &#xA0; Global Execute<br><br>(taken from <a href="http://www.onlamp.com/pub/a/php/2003/02/06/php_foundations.html" rel="nofollow" target="_blank">http://www.onlamp.com/pub/a/php/2003/02/06/php_foundations.html</a>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
In the previous post, stickybit avenger writes:
<br>&#xA0; &#xA0; Just a little hint. I was once adwised to set the &apos;sticky bit&apos;, i.e. use 1777 as chmod-value...
<br>
<br>Note that in order to set the sticky bit on a file one must use &apos;01777&apos; (oct) and not &apos;1777&apos; (dec) as the parameter to chmod:
<br>
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0; chmod</span><span class="keyword">(</span><span class="string">&quot;file&quot;</span><span class="keyword">,</span><span class="default">01777</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">// correct
<br>&#xA0; &#xA0;&#xA0; </span><span class="default">chmod</span><span class="keyword">(</span><span class="string">&quot;file&quot;</span><span class="keyword">,</span><span class="default">1777</span><span class="keyword">);&#xA0; &#xA0; </span><span class="comment">// incorrect, same as chmod(&quot;file&quot;,01023), causing no owner permissions!
<br></span><span class="default">?&gt;
<br></span>
<br>Rule of thumb: always prefix octal mode values with a zero.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Changes file mode recursive in $pathname to $filemode<br><br><span class="default">&lt;?php<br><br>$iterator </span><span class="keyword">= new </span><span class="default">RecursiveIteratorIterator</span><span class="keyword">(new </span><span class="default">RecursiveDirectoryIterator</span><span class="keyword">(</span><span class="default">$pathname</span><span class="keyword">));<br><br>foreach(</span><span class="default">$iterator </span><span class="keyword">as </span><span class="default">$item</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">chmod</span><span class="keyword">(</span><span class="default">$item</span><span class="keyword">, </span><span class="default">$filemode</span><span class="keyword">);<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.chmod.php)

**[To root](/README.md)**