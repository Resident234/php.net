# chr




<div class="phpcode"><span class="html">
Note that if the number is higher than 256, it will return the number mod 256.<br>For example :<br>chr(321)=A because A=65(256)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Another quick and short function to get unicode char by its code.<br><br><span class="default">&lt;?php<br></span><span class="comment">/**<br> * Return unicode char by its code<br> *<br> * @param int $u<br> * @return char<br> */<br></span><span class="keyword">function </span><span class="default">unichr</span><span class="keyword">(</span><span class="default">$u</span><span class="keyword">) {<br>&#xA0; &#xA0; return </span><span class="default">mb_convert_encoding</span><span class="keyword">(</span><span class="string">&apos;&amp;#&apos; </span><span class="keyword">. </span><span class="default">intval</span><span class="keyword">(</span><span class="default">$u</span><span class="keyword">) . </span><span class="string">&apos;;&apos;</span><span class="keyword">, </span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">, </span><span class="string">&apos;HTML-ENTITIES&apos;</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.chr.php)

**[⬆ to root](/)**