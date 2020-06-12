# Prepared statements and stored procedures




<div class="phpcode"><span class="html">
Note that when using name parameters with bindParam, the name itself, cannot contain a dash &apos;-&apos;. <br><br>example:<br><span class="default">&lt;?php<br>$stmt </span><span class="keyword">= </span><span class="default">$dbh</span><span class="keyword">-&gt;</span><span class="default">prepare </span><span class="keyword">(</span><span class="string">&quot;INSERT INTO user (firstname, surname) VALUES (:f-name, :s-name)&quot;</span><span class="keyword">);<br></span><span class="default">$stmt </span><span class="keyword">-&gt; </span><span class="default">bindParam</span><span class="keyword">(</span><span class="string">&apos;:f-name&apos;</span><span class="keyword">, </span><span class="string">&apos;John&apos;</span><span class="keyword">);<br></span><span class="default">$stmt </span><span class="keyword">-&gt; </span><span class="default">bindParam</span><span class="keyword">(</span><span class="string">&apos;:s-name&apos;</span><span class="keyword">, </span><span class="string">&apos;Smith&apos;</span><span class="keyword">);<br></span><span class="default">$stmt </span><span class="keyword">-&gt; </span><span class="default">execute</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br>The dashes in &apos;f-name&apos; and &apos;s-name&apos; should be replaced with an underscore or no dash at all.<br><br>See <a href="http://bugs.php.net/43130" rel="nofollow" target="_blank">http://bugs.php.net/43130</a><br><br>Adam</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdo.prepared-statements.php)

**[To root](/README.md)**