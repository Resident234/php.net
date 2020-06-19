# PDOStatement::fetch




<div class="phpcode"><span class="html">
WARNING:<br>fetch() does NOT adhere to SQL-92 SQLSTATE standard when dealing with empty datasets.<br><br>Instead of setting the errorcode class to 20 to indicate &quot;no data found&quot;, it returns a class of 00 indicating success, and returns NULL to the caller.<br><br>This also prevents the exception mechainsm from firing.<br><br>Programmers will need to explicitly code tests for empty resultsets after any fetch*() instead of relying on the default behavior of the RDBMS.<br><br>I tried logging this as a bug, but it was dismissed as &quot;working as intended&quot;. Just a head&apos;s up.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If no record, this function will also return false.<br>I think that is not very good...</span>
</div>
  

#


<div class="phpcode"><span class="html">
Someone&apos;s already pointed out that PDO::CURSOR_SCROLL isn&apos;t supported by the SQLite driver. It&apos;s also worth noting that it&apos;s not supported by the MySQL driver either.<br><br>In fact, if you try to use scrollable cursors with a MySQL statement, the PDO::FETCH_ORI_ABS parameter and the offset given to fetch() will be silently ignored. fetch() will behave as normal, returning rows in the order in which they came out of the database.<br><br>It&apos;s actually pretty confusing behaviour at first. Definitely worth documenting even if only as a user-added note on this page.</span>
</div>
  

#


<div class="phpcode"><span class="html">
When using PDO::FETCH_COLUMN in a while loop, it&apos;s not enough to just use the value in the while statement as many examples show:<br><br><span class="default">&lt;?php<br></span><span class="keyword">while (</span><span class="default">$row </span><span class="keyword">= </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">fetch</span><span class="keyword">(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_COLUMN</span><span class="keyword">)) {<br>&#xA0; &#xA0; print </span><span class="default">$row</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>If there are 5 rows with values 1 2 0 4 5, then the while loop above will stop at the third row printing only 1 2. The solution is to either explicitly test for false:<br><br><span class="default">&lt;?php<br></span><span class="keyword">while ((</span><span class="default">$row </span><span class="keyword">= </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">fetch</span><span class="keyword">(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_COLUMN</span><span class="keyword">)) !== </span><span class="default">false</span><span class="keyword">) {<br>&#xA0; &#xA0; print </span><span class="default">$row</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>Or use foreach with fetchAll():<br><br><span class="default">&lt;?php<br></span><span class="keyword">foreach (</span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">fetchAll</span><span class="keyword">(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_COLUMN</span><span class="keyword">) as </span><span class="default">$row</span><span class="keyword">) {<br>&#xA0; &#xA0; print </span><span class="default">$row</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>Both will correctly print 1 2 0 4 5.</span>
</div>
  

#


<div class="phpcode"><span class="html">
When fetching an object, the constructor of the class is called after the fields are populated by default.
<br>
<br>PDO::FETCH_PROPS_LATE is used to change the behaviour and make it work as expected - constructor be called _before_ the object fields will be populated with the data.
<br>
<br>sample:
<br>
<br><span class="default">&lt;?php
<br>$a </span><span class="keyword">= </span><span class="default">$PDO</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&apos;select id from table&apos;</span><span class="keyword">);
<br></span><span class="default">$a</span><span class="keyword">-&gt;</span><span class="default">setFetchMode</span><span class="keyword">(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_CLASS</span><span class="keyword">|</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_PROPS_LATE</span><span class="keyword">, </span><span class="string">&apos;ClassName&apos;</span><span class="keyword">);
<br></span><span class="default">$obj </span><span class="keyword">= </span><span class="default">$a</span><span class="keyword">-&gt;</span><span class="default">fetch</span><span class="keyword">();
<br></span><span class="default">?&gt;
<br></span>
<br><a href="http://bugs.php.net/bug.php?id=53394" rel="nofollow" target="_blank">http://bugs.php.net/bug.php?id=53394</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
A quick one liner to get the first entry returned.&#xA0; This is nice for very basic queries.<br><br><span class="default">&lt;?php<br>$count </span><span class="keyword">= </span><span class="default">current</span><span class="keyword">(</span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;select count(*) from table&quot;</span><span class="keyword">)-&gt;</span><span class="default">fetch</span><span class="keyword">());<br></span><span class="default">?&gt;</span>php</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.fetch.php)

**[To root](/README.md)**