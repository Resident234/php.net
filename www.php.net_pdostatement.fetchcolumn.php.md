# PDOStatement::fetchColumn




<div class="phpcode"><span class="html">
fetchColumn return boolean false when a row not is found or don&apos;t had more rows.</span>
</div>
  

#


<div class="phpcode"><span class="html">
This is an excellent method for returning a column count. For example:
<br>
<br><span class="default">&lt;?php
<br>$db </span><span class="keyword">= new </span><span class="default">PDO</span><span class="keyword">(</span><span class="string">&apos;mysql:host=localhost;dbname=pictures&apos;</span><span class="keyword">,</span><span class="string">&apos;user&apos;</span><span class="keyword">,</span><span class="string">&apos;password&apos;</span><span class="keyword">);
<br></span><span class="default">$pics </span><span class="keyword">= </span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&apos;SELECT COUNT(id) FROM pics&apos;</span><span class="keyword">);
<br></span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">totalpics </span><span class="keyword">= </span><span class="default">$pics</span><span class="keyword">-&gt;</span><span class="default">fetchColumn</span><span class="keyword">();
<br></span><span class="default">$db </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>In my case $pics-&gt;fetchColumn() returns 641 because that is how many pictures I have in my db.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.fetchcolumn.php)

**[To root](/README.md)**