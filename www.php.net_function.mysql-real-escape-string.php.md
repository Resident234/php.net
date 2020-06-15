# mysql_real_escape_stringSecurity: the default character set




<div class="phpcode"><span class="html">
Just a little function which mimics the original mysql_real_escape_string but which doesn&apos;t need an active mysql connection. Could be implemented as a static function in a database class. Hope it helps someone.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">mysql_escape_mimic</span><span class="keyword">(</span><span class="default">$inp</span><span class="keyword">) {
<br>&#xA0; &#xA0; if(</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$inp</span><span class="keyword">))
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">array_map</span><span class="keyword">(</span><span class="default">__METHOD__</span><span class="keyword">, </span><span class="default">$inp</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; if(!empty(</span><span class="default">$inp</span><span class="keyword">) &amp;&amp; </span><span class="default">is_string</span><span class="keyword">(</span><span class="default">$inp</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">str_replace</span><span class="keyword">(array(</span><span class="string">&apos;\\&apos;</span><span class="keyword">, </span><span class="string">&quot;\0&quot;</span><span class="keyword">, </span><span class="string">&quot;\n&quot;</span><span class="keyword">, </span><span class="string">&quot;\r&quot;</span><span class="keyword">, </span><span class="string">&quot;&apos;&quot;</span><span class="keyword">, </span><span class="string">&apos;&quot;&apos;</span><span class="keyword">, </span><span class="string">&quot;\x1a&quot;</span><span class="keyword">), array(</span><span class="string">&apos;\\\\&apos;</span><span class="keyword">, </span><span class="string">&apos;\\0&apos;</span><span class="keyword">, </span><span class="string">&apos;\\n&apos;</span><span class="keyword">, </span><span class="string">&apos;\\r&apos;</span><span class="keyword">, </span><span class="string">&quot;\\&apos;&quot;</span><span class="keyword">, </span><span class="string">&apos;\\&quot;&apos;</span><span class="keyword">, </span><span class="string">&apos;\\Z&apos;</span><span class="keyword">), </span><span class="default">$inp</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; return </span><span class="default">$inp</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that mysql_real_escape_string doesn&apos;t prepend backslashes to \x00, \n, \r, and and \x1a as mentionned in the documentation, but actually replaces the character with a MySQL acceptable representation for queries (e.g. \n is replaced with the &apos;\n&apos; litteral). (\, &apos;, and &quot; are escaped as documented) This doesn&apos;t change how you should use this function, but I think it&apos;s good to know.</span>
</div>
  

#


<div class="phpcode"><span class="html">
For further information:<br><a href="http://dev.mysql.com/doc/refman/5.5/en/mysql-real-escape-string.html" rel="nofollow" target="_blank">http://dev.mysql.com/doc/refman/5.5/en/mysql-real-escape-string.html</a><br>(replace your MySQL version in the URL)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.mysql-real-escape-string.php)

**[To root](/README.md)**