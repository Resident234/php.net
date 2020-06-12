# Microsoft SQL Server




<div class="phpcode"><span class="html">
Hi, i was need some short and simple script to list all tables and columns of MSSQL database. There was nothing easy to explain on the net, so i&apos;ve decided to share my short script, i hope it will help.
<br>
<br><span class="default">&lt;?php
<br>$all </span><span class="keyword">= </span><span class="default">MSSQL_Query</span><span class="keyword">(</span><span class="string">&quot;select TABLE_NAME, COLUMN_NAME from INFORMATION_SCHEMA.COLUMNS order by TABLE_NAME, ORDINAL_POSITION&quot;</span><span class="keyword">);
<br>
<br></span><span class="default">$tables </span><span class="keyword">= array();
<br></span><span class="default">$columns </span><span class="keyword">= array();
<br>
<br>while(</span><span class="default">$fet_tbl </span><span class="keyword">= </span><span class="default">MSSQL_Fetch_Assoc</span><span class="keyword">(</span><span class="default">$all</span><span class="keyword">)) { </span><span class="comment">// PUSH ALL TABLES AND COLUMNS INTO THE ARRAY
<br>
<br>&#xA0; </span><span class="default">$tables</span><span class="keyword">[] = </span><span class="default">$fet_tbl</span><span class="keyword">[</span><span class="default">TABLE_NAME</span><span class="keyword">];
<br>&#xA0; </span><span class="default">$columns</span><span class="keyword">[] = </span><span class="default">$fet_tbl</span><span class="keyword">[</span><span class="default">COLUMN_NAME</span><span class="keyword">];
<br>
<br>}
<br>
<br></span><span class="default">$sltml </span><span class="keyword">= </span><span class="default">array_count_values</span><span class="keyword">(</span><span class="default">$tables</span><span class="keyword">); </span><span class="comment">// HOW MANY COLUMNS ARE IN THE TABLE
<br>
<br></span><span class="keyword">foreach(</span><span class="default">$sltml </span><span class="keyword">as </span><span class="default">$table_name </span><span class="keyword">=&gt; </span><span class="default">$id</span><span class="keyword">) {
<br> 
<br> echo </span><span class="string">&quot;&lt;h2&gt;&quot;</span><span class="keyword">. </span><span class="default">$table_name </span><span class="keyword">.</span><span class="string">&quot; (&quot;</span><span class="keyword">. </span><span class="default">$id </span><span class="keyword">.</span><span class="string">&quot;)&lt;/h2&gt;&lt;ol&gt;&quot;</span><span class="keyword">;
<br> 
<br>&#xA0; &#xA0; for(</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt;= </span><span class="default">$id</span><span class="keyword">-</span><span class="default">1</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++) {
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; echo </span><span class="string">&quot;&lt;li&gt;&quot;</span><span class="keyword">. </span><span class="default">$columns</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">] .</span><span class="string">&quot;&lt;/li&gt;&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; echo</span><span class="string">&quot;&lt;/ol&gt;&quot;</span><span class="keyword">;
<br> 
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/book.mssql.php)

**[⬆ to root](/)**