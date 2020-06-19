# PDO::quote




<div class="phpcode"><span class="html">
When converting from the old mysql_ functions to PDO, note that the quote function isn&apos;t exactly the same as the old mysql_real_escape_string function. It escapes, but also adds quotes; hence the name I guess :-)<br><br>After I replaced mysql_real_escape_string with $pdo-&gt;quote, it took me a bit to figure out why my strings were turning up in results with quotes around them. I felt like a fool when I realized all I needed to do was change ...\&quot;&quot;.$pdo-&gt;quote($foo).&quot;\&quot;... to ...&quot;.$pdo-&gt;quote($foo).&quot;...</span>
</div>
  

#


<div class="phpcode"><span class="html">
PDO quote (tested with mysql and mariadb 10.3) is extremely slow.<br><br>It took me hours of debugging my performance issues until I found that pdo-&gt;quote is the problem.<br><br>This function is far from fast, and it&apos;s PHP instead of C code:<br>function escape($value)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; $search = array(&quot;\\&quot;,&#xA0; &quot;\x00&quot;, &quot;\n&quot;,&#xA0; &quot;\r&quot;,&#xA0; &quot;&apos;&quot;,&#xA0; &apos;&quot;&apos;, &quot;\x1a&quot;);<br>&#xA0; &#xA0; &#xA0; &#xA0; $replace = array(&quot;\\\\&quot;,&quot;\\0&quot;,&quot;\\n&quot;, &quot;\\r&quot;, &quot;\&apos;&quot;, &apos;\&quot;&apos;, &quot;\\Z&quot;);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; return str_replace($search, $replace, $value);<br>&#xA0; &#xA0; }<br><br>It is 50 times faster than pdo-&gt;quote()<br>(note, it&apos;s without quotes just escaping and only used here as an example)</span>
</div>
  

#


<div class="phpcode"><span class="html">
One have to understand that string formatting has nothing to do with identifiers.<br>And thus string formatting should NEVER ever be used to format an identifier ( table of field name).<br>To quote an identifier, you have to format it as identifier, not as string.<br>To do so you have to<br><br>- Enclose identifier in backticks.<br>- Escape backticks inside by doubling them.<br><br>So, the code would be:<br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">quoteIdent</span><span class="keyword">(</span><span class="default">$field</span><span class="keyword">) {<br>&#xA0; &#xA0; return </span><span class="string">&quot;`&quot;</span><span class="keyword">.</span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&quot;`&quot;</span><span class="keyword">,</span><span class="string">&quot;``&quot;</span><span class="keyword">,</span><span class="default">$field</span><span class="keyword">).</span><span class="string">&quot;`&quot;</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span>this will make your identifier properly formatted and thus invulnerable to injection. <br><br>However, there is another possible attack vector - using dynamical identifiers in the query may give an outsider control over fields the aren&apos;t allowed to:<br>Say, a field user_role in the users table and a dynamically built INSERT query based on a $_POST array may allow a privilege escalation with easily forged $_POST array. <br>Or a select query which let a user to choose fields to display may reveal some sensitive information to attacker.<br><br>To prevent this kind of attack yet keep queries dynamic, one ought to use WHITELISTING approach.<br><br>Every dynamical identifier have to be checked against a hardcoded whitelist like this:<br><span class="default">&lt;?php<br>$allowed&#xA0; </span><span class="keyword">= array(</span><span class="string">&quot;name&quot;</span><span class="keyword">,</span><span class="string">&quot;price&quot;</span><span class="keyword">,</span><span class="string">&quot;qty&quot;</span><span class="keyword">);<br></span><span class="default">$key </span><span class="keyword">= </span><span class="default">array_search</span><span class="keyword">(</span><span class="default">$_GET</span><span class="keyword">[</span><span class="string">&apos;field&apos;</span><span class="keyword">], </span><span class="default">$allowed</span><span class="keyword">));<br>if (</span><span class="default">$key </span><span class="keyword">== </span><span class="default">false</span><span class="keyword">) {<br>&#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&apos;Wrong field name&apos;</span><span class="keyword">);<br>}<br></span><span class="default">$field </span><span class="keyword">= </span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">quoteIdent</span><span class="keyword">(</span><span class="default">$allowed</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]);<br></span><span class="default">$query </span><span class="keyword">= </span><span class="string">&quot;SELECT </span><span class="default">$field</span><span class="string"> FROM t&quot;</span><span class="keyword">; </span><span class="comment">//value is safe<br></span><span class="default">?&gt;<br></span>(Personally I wouldn&apos;t use a query like this, but that&apos;s just an example of using a dynamical identifier in the query).<br><br>And similar approach have to be used when filtering dynamical arrays for insert and update:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">filterArray</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">,</span><span class="default">$allowed</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; foreach(</span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$input</span><span class="keyword">) as </span><span class="default">$key </span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; if ( !</span><span class="default">in_array</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">,</span><span class="default">$allowed</span><span class="keyword">) )<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; unset(</span><span class="default">$input</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return </span><span class="default">$input</span><span class="keyword">;<br>}<br></span><span class="comment">//used like this<br></span><span class="default">$allowed </span><span class="keyword">= array(</span><span class="string">&apos;title&apos;</span><span class="keyword">,</span><span class="string">&apos;url&apos;</span><span class="keyword">,</span><span class="string">&apos;body&apos;</span><span class="keyword">,</span><span class="string">&apos;rating&apos;</span><span class="keyword">,</span><span class="string">&apos;term&apos;</span><span class="keyword">,</span><span class="string">&apos;type&apos;</span><span class="keyword">);<br></span><span class="default">$data </span><span class="keyword">= </span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">filterArray</span><span class="keyword">(</span><span class="default">$_POST</span><span class="keyword">,</span><span class="default">$allowed</span><span class="keyword">); <br></span><span class="comment">// $data now contains allowed fields only <br>// and can be used to create INSERT or UPDATE query dynamically<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
This function also converts new lines to \r\n</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that this function just does what the documentation says: It escapes special characters in strings. <br><br>It does NOT - however - detect a &quot;NULL&quot; value. If the value you try to quote is &quot;NULL&quot; it will return the same value as when you process an empty string (-&gt; &apos;&apos;), not the text &quot;NULL&quot;.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdo.quote.php)

**[To root](/README.md)**