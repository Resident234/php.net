# PDOStatement::bindParam




<div class="phpcode"><span class="html">
I know this has been said before but I&apos;ll write a note on it too because I think it&apos;s important to keep in mind:<br><br>If you use PDO bindParam to do a search with a LIKE condition you cannot put the percentages and quotes to the param placeholder &apos;%:keyword%&apos;.<br><br>This is WRONG:<br>&quot;SELECT * FROM `users` WHERE `firstname` LIKE &apos;%:keyword%&apos;&quot;;<br><br>The CORRECT solution is to leave clean the placeholder like this:<br>&quot;SELECT * FROM `users` WHERE `firstname` LIKE :keyword&quot;;<br><br>And then add the percentages to the php variable where you store the keyword:<br>$keyword = &quot;%&quot;.$keyword.&quot;%&quot;;<br><br>And finally the quotes will be automatically added by PDO when executing the query so you don&apos;t have to worry about them.<br><br>So the full example would be:<br><span class="default">&lt;?php<br></span><span class="comment">// Get the keyword from query string<br></span><span class="default">$keyword </span><span class="keyword">= </span><span class="default">$_GET</span><span class="keyword">[</span><span class="string">&apos;keyword&apos;</span><span class="keyword">];<br></span><span class="comment">// Prepare the command<br></span><span class="default">$sth </span><span class="keyword">= </span><span class="default">$dbh</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="string">&apos;SELECT * FROM `users` WHERE `firstname` LIKE :keyword&apos;</span><span class="keyword">);<br></span><span class="comment">// Put the percentage sing on the keyword<br></span><span class="default">$keyword </span><span class="keyword">= </span><span class="string">&quot;%&quot;</span><span class="keyword">.</span><span class="default">$keyword</span><span class="keyword">.</span><span class="string">&quot;%&quot;</span><span class="keyword">;<br></span><span class="comment">// Bind the parameter<br></span><span class="default">$sth</span><span class="keyword">-&gt;</span><span class="default">bindParam</span><span class="keyword">(</span><span class="string">&apos;:keyword&apos;</span><span class="keyword">, </span><span class="default">$keyword</span><span class="keyword">, </span><span class="default">PDO</span><span class="keyword">::</span><span class="default">PARAM_STR</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
This works ($val by reference):<br><span class="default">&lt;?php<br></span><span class="keyword">foreach (</span><span class="default">$params </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; &amp;</span><span class="default">$val</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$sth</span><span class="keyword">-&gt;</span><span class="default">bindParam</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">, </span><span class="default">$val</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;<br></span><br>This will fail ($val by value, because bindParam needs &amp;$variable):<br><span class="default">&lt;?php<br></span><span class="keyword">foreach (</span><span class="default">$params </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$val</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$sth</span><span class="keyword">-&gt;</span><span class="default">bindParam</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">, </span><span class="default">$val</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that when using PDOStatement::bindParam an integer is changed to a string value upon PDOStatement::execute(). (Tested with MySQL). <br><br>This can cause problems when trying to compare values using the === operator.<br><br>Example:<br><span class="default">&lt;?php<br>$active </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$active</span><span class="keyword">);<br></span><span class="default">$ps</span><span class="keyword">-&gt;</span><span class="default">bindParam</span><span class="keyword">(</span><span class="string">&quot;:active&quot;</span><span class="keyword">, </span><span class="default">$active</span><span class="keyword">, </span><span class="default">PDO</span><span class="keyword">::</span><span class="default">PARAM_INT</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$active</span><span class="keyword">);<br></span><span class="default">$ps</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">();<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$active</span><span class="keyword">);<br>if (</span><span class="default">$active </span><span class="keyword">=== </span><span class="default">1</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="comment">// do something here<br>&#xA0; &#xA0; // note: this will fail since $active is now &quot;1&quot;<br></span><span class="keyword">}<br></span><span class="default">?&gt;<br></span><br>results in:<br>int(1) <br>int(1) <br>string(1) &quot;1&quot;</span>
</div>
  

#


<div class="phpcode"><span class="html">
There seems to be some confusion about whether you can bind a single value to multiple identical placeholders. For example:<br><br>$sql = &quot;SELECT * FROM user WHERE is_admin = :myValue AND is_deleted = :myValue &quot;;<br><br>$params = array(&quot;myValue&quot; =&gt; &quot;0&quot;);<br><br>Some users have reported that attempting to bind a single parameter to multiple placeholders yields a parameter mismatch error in PHP version 5.2.0 and earlier. Starting with version 5.2.1, however, this seems to work just fine.<br><br>For details, see bug report 40417:<br><a href="http://bugs.php.net/bug.php?id=40417" rel="nofollow" target="_blank">http://bugs.php.net/bug.php?id=40417</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
Please note, that PDO format numbers according to current locale. So if, locale set number format to something else, that standard that query WILL NOT work properly.<br><br>For example:<br>in Polish locale (pl_PL) proper decimal separator is coma (&quot;,&quot;), so: 123,45, not 123.45. If we try bind 123.45 to the query, we will end up with coma in the query.<br><br><span class="default">&lt;?php<br>setlocale</span><span class="keyword">(</span><span class="default">LC_ALL</span><span class="keyword">, </span><span class="string">&apos;pl_PL&apos;</span><span class="keyword">);<br></span><span class="default">$sth </span><span class="keyword">= </span><span class="default">$dbh</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="string">&apos;SELECT name FROM products WHERE price &lt; :price&apos;</span><span class="keyword">);<br></span><span class="default">$sth</span><span class="keyword">-&gt;</span><span class="default">bindParam</span><span class="keyword">(</span><span class="string">&apos;:price&apos;</span><span class="keyword">, </span><span class="default">123.45</span><span class="keyword">, </span><span class="default">PDO</span><span class="keyword">::</span><span class="default">PARAM_STR</span><span class="keyword">);<br></span><span class="default">$sth</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">();<br></span><span class="comment">// result:<br>// SELECT name FROM products WHERE price &lt; &apos;123,45&apos;;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Do not try to use the same named parameter twice in a single SQL statement, for example
<br>
<br><span class="default">&lt;?php
<br>$sql </span><span class="keyword">= </span><span class="string">&apos;SELECT * FROM some_table WHERE&#xA0; some_value &gt; :value OR some_value &lt; :value&apos;</span><span class="keyword">;
<br></span><span class="default">$stmt </span><span class="keyword">= </span><span class="default">$dbh</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="default">$sql</span><span class="keyword">);
<br></span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">( array( </span><span class="string">&apos;:value&apos; </span><span class="keyword">=&gt; </span><span class="default">3 </span><span class="keyword">) );
<br></span><span class="default">?&gt;
<br></span>
<br>...this will return no rows and no error -- you must use each parameter once and only once. Apparently this is expected behavior (according to this bug report: <a href="http://bugs.php.net/bug.php?id=33886" rel="nofollow" target="_blank">http://bugs.php.net/bug.php?id=33886</a>)&#xA0; because of portability issues.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.bindparam.php)

**[To root](/README.md)**