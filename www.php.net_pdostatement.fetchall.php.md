# PDOStatement::fetchAll




<div class="phpcode"><span class="html">
I still don&apos;t understand why FETCH_KEY_PAIR is not documented here (<a href="http://php.net/manual/fr/pdo.constants.php" rel="nofollow" target="_blank">http://php.net/manual/fr/pdo.constants.php</a>), because it could be very useful!<br><br><span class="default">&lt;?php<br>&#xA0; var_dump</span><span class="keyword">(</span><span class="default">$pdo</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&apos;select id, name from table&apos;</span><span class="keyword">)-&gt;</span><span class="default">fetchAll</span><span class="keyword">(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_KEY_PAIR</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>This will display:<br>array(2) {<br>&#xA0; [2]=&gt;<br>&#xA0; string(10) &quot;name2&quot;<br>&#xA0; [5]=&gt;<br>&#xA0; string(10) &quot;name5&quot;<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
Getting foreach to play nicely with some data from PDO FetchAll()<br>I was not understanding to use the $value part of the foreach properly, I hope this helps someone else.<br>Example:<br><span class="default">&lt;?php <br>$stmt </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">db</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="string">&apos;SELECT title, FMarticle_id FROM articles WHERE domain_name =:domain_name&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">bindValue</span><span class="keyword">(</span><span class="string">&apos;:domain_name&apos;</span><span class="keyword">, </span><span class="default">$domain</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$article_list </span><span class="keyword">= </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">fetchAll</span><span class="keyword">(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_ASSOC</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span>which gives:<br><br>array (size=2)<br>&#xA0; 0 =&gt; <br>&#xA0; &#xA0; array (size=2)<br>&#xA0; &#xA0; &#xA0; &apos;title&apos; =&gt; string &apos;About Cats Really Long title for the article&apos; (length=44)<br>&#xA0; &#xA0; &#xA0; &apos;FMarticle_id&apos; =&gt; string &apos;7CAEBB15-6784-3A41-909A-1B6D12667499&apos; (length=36)<br>&#xA0; 1 =&gt; <br>&#xA0; &#xA0; array (size=2)<br>&#xA0; &#xA0; &#xA0; &apos;title&apos; =&gt; string &apos;another cat story&apos; (length=17)<br>&#xA0; &#xA0; &#xA0; &apos;FMarticle_id&apos; =&gt; string &apos;0BB86A06-2A79-3145-8A02-ECF6EA5C405C&apos; (length=36)<br><br>Then use:<br><span class="default">&lt;?php<br></span><span class="keyword">foreach (</span><span class="default">$article_list </span><span class="keyword">as </span><span class="default">$row </span><span class="keyword">=&gt; </span><span class="default">$link</span><span class="keyword">) {<br>&#xA0; echo&#xA0; </span><span class="string">&apos;&lt;a href=&quot;&apos;</span><span class="keyword">.&#xA0; </span><span class="default">$link</span><span class="keyword">[</span><span class="string">&apos;FMarticle_id&apos;</span><span class="keyword">].</span><span class="string">&apos;&quot;&gt;&apos; </span><span class="keyword">. </span><span class="default">$link</span><span class="keyword">[</span><span class="string">&apos;title&apos;</span><span class="keyword">]. </span><span class="string">&apos;&lt;/a&gt;&lt;/br&gt;&apos;</span><span class="keyword">;<br>&#xA0; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
You might find yourself wanting to use FETCH_GROUP and FETCH_ASSOC at the same time, to get your table&apos;s primary key as the array key:<br><span class="default">&lt;?php<br></span><span class="comment">// $stmt is some query like &quot;SELECT rowid, username, comment&quot;<br></span><span class="default">$results </span><span class="keyword">= </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">fetchAll</span><span class="keyword">(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_GROUP</span><span class="keyword">|</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_ASSOC</span><span class="keyword">);<br><br></span><span class="comment">// It does work, but not as you might expect:<br></span><span class="default">$results </span><span class="keyword">= array(<br>&#xA0; &#xA0; </span><span class="default">1234 </span><span class="keyword">=&gt; array(</span><span class="default">0 </span><span class="keyword">=&gt; array(</span><span class="string">&apos;username&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;abc&apos;</span><span class="keyword">, </span><span class="string">&apos;comment&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;[...]&apos;</span><span class="keyword">)),<br>&#xA0; &#xA0; </span><span class="default">1235 </span><span class="keyword">=&gt; array(</span><span class="default">0 </span><span class="keyword">=&gt; array(</span><span class="string">&apos;username&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;def&apos;</span><span class="keyword">, </span><span class="string">&apos;comment&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;[...]&apos;</span><span class="keyword">)),<br>);<br><br></span><span class="comment">// ...but you can at least strip the useless numbered array out easily:<br></span><span class="default">$results </span><span class="keyword">= </span><span class="default">array_map</span><span class="keyword">(</span><span class="string">&apos;reset&apos;</span><span class="keyword">, </span><span class="default">$results</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Interestingly enough, when you use fetchAll, the constructor for your object is called AFTER the properties are assigned. For example:<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">person </span><span class="keyword">{<br>&#xA0; &#xA0; public </span><span class="default">$name</span><span class="keyword">;<br><br>&#xA0; &#xA0; function </span><span class="default">__construct</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">name </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">name </span><span class="keyword">. </span><span class="string">&quot; is my name.&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="comment"># set up select from a database here with PDO<br></span><span class="default">$obj </span><span class="keyword">= </span><span class="default">$STH</span><span class="keyword">-&gt;</span><span class="default">fetchALL</span><span class="keyword">(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_CLASS</span><span class="keyword">, </span><span class="string">&apos;person&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Will result in &apos; is my name&apos; being appended to all the name columns. However if you call it slightly differently:<br><br><span class="default">&lt;?php<br>$obj </span><span class="keyword">= </span><span class="default">$obj </span><span class="keyword">= </span><span class="default">$STH</span><span class="keyword">-&gt;</span><span class="default">fetchAll</span><span class="keyword">(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_CLASS </span><span class="keyword">| </span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_PROPS_LATE</span><span class="keyword">, </span><span class="string">&apos;person&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Then the constructor will be called before properties are assigned. I can&apos;t find this documented anywhere, so I thought it would be nice to add a note here.</span>
</div>
  

#


<div class="phpcode"><span class="html">
PLEASE BE AWARE: If you do an OUTER LEFT JOIN and set PDO FetchALL to PDO::FETCH_ASSOC, any primary key you used in the OUTER LEFT JOIN will be set to a blank if there are no records returned in the JOIN.<br><br>For example:<br><span class="default">&lt;?php<br></span><span class="comment">//query the product table and join to the image table and return any images, if we have any, for each product<br></span><span class="default">$sql </span><span class="keyword">= </span><span class="string">&quot;SELECT * FROM product, image<br>LEFT OUTER JOIN image ON (product.product_id = image.product_id)&quot;</span><span class="keyword">;<br><br></span><span class="default">$array </span><span class="keyword">= </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">fetchAll</span><span class="keyword">(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_ASSOC</span><span class="keyword">);<br><br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>The resulting array will look something like this:<br><br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [product_id] =&gt; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [notes] =&gt; &quot;this product...&quot;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [brand] =&gt; &quot;Best Yet&quot;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ...<br><br>The fix is to simply specify your field names in the SELECT clause instead of using the * as a wild card, or, you can also specify the field in addition to the *. The following example returns the product_id field correctly:<br><br><span class="default">&lt;?php<br>$sql </span><span class="keyword">= </span><span class="string">&quot;SELECT *, product.product_id FROM product, image<br>LEFT OUTER JOIN image ON (product.product_id = image.product_id)&quot;</span><span class="keyword">;<br><br></span><span class="default">$array </span><span class="keyword">= </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">fetchAll</span><span class="keyword">(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_ASSOC</span><span class="keyword">);<br><br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$array</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>The resulting array will look something like this:<br><br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [product_id] =&gt; 3<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [notes] =&gt; &quot;this product...&quot;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [brand] =&gt; &quot;Best Yet&quot;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ...</span>
</div>
  

#


<div class="phpcode"><span class="html">
to fetch rows grouped by primary id or any other field you may use FETCH_GROUP with FETCH_UNIQUE:<br><br><span class="default">&lt;?php<br><br></span><span class="comment">//prepare and execute a statement returning multiple rows, on a single one<br></span><span class="default">$stmt </span><span class="keyword">= </span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="string">&apos;SELECT id,name,role FROM table&apos;</span><span class="keyword">);<br></span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">();<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">fetchAll</span><span class="keyword">(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_GROUP </span><span class="keyword">| </span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_UNIQUE</span><span class="keyword">));<br><br></span><span class="comment">//returns an array with the first selected field as key containing associative arrays with the row. This mode takes care not to repeat the key in corresponding grouped array.<br><br></span><span class="default">$result </span><span class="keyword">= array<br> (</span><span class="default">1 </span><span class="keyword">=&gt; array<br>&#xA0;&#xA0; (</span><span class="string">&apos;name&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;foo&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;role&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;sage&apos;</span><span class="keyword">,),<br>&#xA0; </span><span class="default">2 </span><span class="keyword">=&gt; array<br>&#xA0;&#xA0; (</span><span class="string">&apos;name&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;bar&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;role&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;rage&apos;</span><span class="keyword">,),);<br><br></span><span class="comment">// &apos;SELECT name,id,role FROM table&apos; would result in that:<br><br></span><span class="default">$result </span><span class="keyword">= array<br> (</span><span class="string">&apos;foo&apos; </span><span class="keyword">=&gt; array<br>&#xA0;&#xA0; (</span><span class="string">&apos;id&apos;</span><span class="keyword">=&gt;</span><span class="default">1</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;role&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;sage&apos;</span><span class="keyword">,),<br>&#xA0; </span><span class="string">&apos;bar&apos; </span><span class="keyword">=&gt; array<br>&#xA0;&#xA0; (</span><span class="string">&apos;id&apos;</span><span class="keyword">=&gt;</span><span class="default">2</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;role&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;rage&apos;</span><span class="keyword">,),);<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If no rows have been returned, fetchAll returns an empty array.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that fetchAll() can be extremely memory inefficient for large data sets. My memory limit was set to 160 MB this is what happened when I tried:
<br>
<br><span class="default">&lt;?php
<br>$arr </span><span class="keyword">= </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">fetchAll</span><span class="keyword">();
<br></span><span class="comment">// Fatal error: Allowed memory size of 16777216 bytes exhausted
<br></span><span class="default">?&gt;
<br></span>
<br>If you are going to loop through the output array of fetchAll(), instead use fetch() to minimize memory usage as follows:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">while (</span><span class="default">$arr </span><span class="keyword">= </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">fetch</span><span class="keyword">()) {
<br>&#xA0; &#xA0; echo </span><span class="default">round</span><span class="keyword">(</span><span class="default">memory_get_usage</span><span class="keyword">() / (</span><span class="default">1024</span><span class="keyword">*</span><span class="default">1024</span><span class="keyword">),</span><span class="default">3</span><span class="keyword">) .</span><span class="string">&apos; MB&lt;br /&gt;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="comment">// do_other_stuff();
<br></span><span class="keyword">}
<br></span><span class="comment">// Last line for the same query shows only 28.973 MB usage
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
There is also another fetch mode supported on Oracle and MSSQL: <br>PDO::FETCH_ASSOC<br><br>&gt; fetches only column names and omits the numeric index.<br><br>If you would like to return all columns from an sql statement with column keys as table headers, it&apos;s as simple as this:<br><br><span class="default">&lt;?php<br>$dbh </span><span class="keyword">= new </span><span class="default">PDO</span><span class="keyword">(</span><span class="string">&quot;DS&quot;</span><span class="keyword">, </span><span class="string">&quot;USERNAME&quot;</span><span class="keyword">, </span><span class="string">&quot;PASSWORD&quot;</span><span class="keyword">);<br></span><span class="default">$stmt </span><span class="keyword">= </span><span class="default">$dbh</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="string">&quot;SELECT * FROM tablename&quot;</span><span class="keyword">);<br></span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">();<br></span><span class="default">$arrValues </span><span class="keyword">= </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">fetchAll</span><span class="keyword">(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_ASSOC</span><span class="keyword">);<br></span><span class="comment">// open the table<br></span><span class="keyword">print </span><span class="string">&quot;&lt;table wdith=\&quot;100%\&quot;&gt;\n&quot;</span><span class="keyword">;<br>print </span><span class="string">&quot;&lt;tr&gt;\n&quot;</span><span class="keyword">;<br></span><span class="comment">// add the table headers<br></span><span class="keyword">foreach (</span><span class="default">$arrValues</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$useless</span><span class="keyword">){<br>&#xA0; &#xA0; print </span><span class="string">&quot;&lt;th&gt;</span><span class="default">$key</span><span class="string">&lt;/th&gt;&quot;</span><span class="keyword">;<br>}<br>print </span><span class="string">&quot;&lt;/tr&gt;&quot;</span><span class="keyword">;<br></span><span class="comment">// display data<br></span><span class="keyword">foreach (</span><span class="default">$arrValues </span><span class="keyword">as </span><span class="default">$row</span><span class="keyword">){<br>&#xA0; &#xA0; print </span><span class="string">&quot;&lt;tr&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; foreach (</span><span class="default">$row </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$val</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; print </span><span class="string">&quot;&lt;td&gt;</span><span class="default">$val</span><span class="string">&lt;/td&gt;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; print </span><span class="string">&quot;&lt;/tr&gt;\n&quot;</span><span class="keyword">;<br>}<br></span><span class="comment">// close the table<br></span><span class="keyword">print </span><span class="string">&quot;&lt;/table&gt;\n&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.fetchall.php)

**[To root](/README.md)**