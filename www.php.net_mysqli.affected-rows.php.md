# mysqli::$affected_rows




<div class="phpcode"><span class="html">
On &quot;INSERT INTO ON DUPLICATE KEY UPDATE&quot; queries, though one may expect affected_rows to return only 0 or 1 per row on successful queries, it may in fact return 2.<br><br>From Mysql manual: &quot;With ON DUPLICATE KEY UPDATE, the affected-rows value per row is 1 if the row is inserted as a new row and 2 if an existing row is updated.&quot;<br><br>See: <a href="http://dev.mysql.com/doc/refman/5.0/en/insert-on-duplicate.html" rel="nofollow" target="_blank">http://dev.mysql.com/doc/refman/5.0/en/insert-on-duplicate.html</a><br><br>Here&apos;s the sum breakdown _per row_:<br>+0: a row wasn&apos;t updated or inserted (likely because the row already existed, but no field values were actually changed during the UPDATE)<br>+1: a row was inserted<br>+2: a row was updated</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you need to know specifically whether the WHERE condition of an UPDATE operation failed to match rows, or that simply no rows required updating you need to instead check mysqli::$info.<br><br>As this returns a string that requires parsing, you can use the following to convert the results into an associative array.<br><br>Object oriented style:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; preg_match_all </span><span class="keyword">(</span><span class="string">&apos;/(\S[^:]+): (\d+)/&apos;</span><span class="keyword">, </span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">info</span><span class="keyword">, </span><span class="default">$matches</span><span class="keyword">); <br>&#xA0; &#xA0; </span><span class="default">$info </span><span class="keyword">= </span><span class="default">array_combine </span><span class="keyword">(</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">], </span><span class="default">$matches</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">]);<br></span><span class="default">?&gt;<br></span><br>Procedural style:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; preg_match_all </span><span class="keyword">(</span><span class="string">&apos;/(\S[^:]+): (\d+)/&apos;</span><span class="keyword">, </span><span class="default">mysqli_info </span><span class="keyword">(</span><span class="default">$link</span><span class="keyword">), </span><span class="default">$matches</span><span class="keyword">); <br>&#xA0; &#xA0; </span><span class="default">$info </span><span class="keyword">= </span><span class="default">array_combine </span><span class="keyword">(</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">], </span><span class="default">$matches</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">]);<br></span><span class="default">?&gt;<br></span><br>You can then use the array to test for the different conditions<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">$info </span><span class="keyword">[</span><span class="string">&apos;Rows matched&apos;</span><span class="keyword">] == </span><span class="default">0</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;This operation did not match any rows.\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; } elseif (</span><span class="default">$info </span><span class="keyword">[</span><span class="string">&apos;Changed&apos;</span><span class="keyword">] == </span><span class="default">0</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;This operation matched rows, but none required updating.\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; if (</span><span class="default">$info </span><span class="keyword">[</span><span class="string">&apos;Changed&apos;</span><span class="keyword">] &lt; </span><span class="default">$info </span><span class="keyword">[</span><span class="string">&apos;Rows matched&apos;</span><span class="keyword">]) {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo (</span><span class="default">$info </span><span class="keyword">[</span><span class="string">&apos;Rows matched&apos;</span><span class="keyword">] - </span><span class="default">$info </span><span class="keyword">[</span><span class="string">&apos;Changed&apos;</span><span class="keyword">]).</span><span class="string">&quot; rows matched but were not changed.\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;<br></span><br>This approach can be used with any query that mysqli::$info supports (INSERT INTO, LOAD DATA, ALTER TABLE, and UPDATE), for other any queries it returns an empty array.<br><br>For any UPDATE operation the array returned will have the following elements:<br><br>Array<br>(<br>&#xA0; &#xA0; [Rows matched] =&gt; 1<br>&#xA0; &#xA0; [Changed] =&gt; 0<br>&#xA0; &#xA0; [Warnings] =&gt; 0<br>)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.affected-rows.php)

**[To root](/README.md)**