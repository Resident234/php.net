# mysqli_stmt::bind_parammysqli_stmt_bind_param




<div class="phpcode"><span class="html">
There are some things to note when working with mysqli::bind_param() and array-elements.
<br>Re-assigning an array will break any references, no matter if the keys are identical.
<br>You have to explicitly reassign every single value in an array, for the references to be kept.
<br>Best shown in an example:
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">getData</span><span class="keyword">() {
<br>&#xA0; &#xA0; return array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">0</span><span class="keyword">=&gt;array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;name&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;test_0&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;email&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;test_0@example.com&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">),
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">1</span><span class="keyword">=&gt;array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;name&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;test_1&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;email&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;test_1@example.com&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">)
<br>&#xA0; &#xA0; );
<br>}
<br></span><span class="default">$db&#xA0; </span><span class="keyword">= new </span><span class="default">mysqli</span><span class="keyword">(</span><span class="string">&quot;localhost&quot;</span><span class="keyword">,</span><span class="string">&quot;root&quot;</span><span class="keyword">,</span><span class="string">&quot;&quot;</span><span class="keyword">,</span><span class="string">&quot;tests&quot;</span><span class="keyword">);
<br></span><span class="default">$sql </span><span class="keyword">= </span><span class="string">&quot;INSERT INTO `user` SET `name`=?,`email`=?&quot;</span><span class="keyword">;
<br></span><span class="default">$res </span><span class="keyword">= </span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="default">$sql</span><span class="keyword">);
<br></span><span class="comment">// If you bind array-elements to a prepared statement, the array has to be declared first with the used keys:
<br></span><span class="default">$arr </span><span class="keyword">= array(</span><span class="string">&quot;name&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;&quot;</span><span class="keyword">,</span><span class="string">&quot;email&quot;</span><span class="keyword">=&gt;</span><span class="string">&quot;&quot;</span><span class="keyword">); 
<br></span><span class="default">$res</span><span class="keyword">-&gt;</span><span class="default">bind_param</span><span class="keyword">(</span><span class="string">&quot;ss&quot;</span><span class="keyword">,</span><span class="default">$arr</span><span class="keyword">[</span><span class="string">&apos;name&apos;</span><span class="keyword">],</span><span class="default">$arr</span><span class="keyword">[</span><span class="string">&apos;email&apos;</span><span class="keyword">]);
<br></span><span class="comment">//So far the introduction...
<br>
<br>/* 
<br>&#xA0; &#xA0; Example 1 (wont work as expected, creates two empty entries)
<br>&#xA0; &#xA0; Re-assigning the array in the while()-head generates a new array, whereas references from bind_param stick to the old array
<br>*/
<br></span><span class="keyword">foreach( </span><span class="default">getData</span><span class="keyword">() as </span><span class="default">$arr </span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$res</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">();
<br>}
<br>
<br></span><span class="comment">/*
<br>&#xA0; &#xA0; Example 2 (will work as expected)
<br>&#xA0; &#xA0; Re-assigning every single value explicitly keeps the references alive
<br>*/
<br></span><span class="keyword">foreach( </span><span class="default">getData</span><span class="keyword">() as </span><span class="default">$tempArr </span><span class="keyword">) {
<br>&#xA0; &#xA0; foreach(</span><span class="default">$tempArr </span><span class="keyword">as </span><span class="default">$k</span><span class="keyword">=&gt;</span><span class="default">$v</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$arr</span><span class="keyword">[</span><span class="default">$k</span><span class="keyword">] = </span><span class="default">$v</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; </span><span class="default">$res</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">();
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Coming to the problem calling mysqli::bind_param() with a dynamic number of arguments via call_user_func_array() with PHP Version 5.3+, there&apos;s another workaround besides using an extra function to build the references for the array-elements.
<br>You can use Reflection to call mysqli::bind_param(). When using PHP 5.3+ this saves you about 20-40% Speed compared to passing the array to your own reference-builder-function.
<br>Example:
<br><span class="default">&lt;?php
<br>$db&#xA0; &#xA0;&#xA0; </span><span class="keyword">= new </span><span class="default">mysqli</span><span class="keyword">(</span><span class="string">&quot;localhost&quot;</span><span class="keyword">,</span><span class="string">&quot;root&quot;</span><span class="keyword">,</span><span class="string">&quot;&quot;</span><span class="keyword">,</span><span class="string">&quot;tests&quot;</span><span class="keyword">); 
<br></span><span class="default">$res&#xA0; &#xA0; </span><span class="keyword">= </span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="string">&quot;INSERT INTO test SET foo=?,bar=?&quot;</span><span class="keyword">); 
<br></span><span class="default">$refArr </span><span class="keyword">= array(</span><span class="string">&quot;si&quot;</span><span class="keyword">,</span><span class="string">&quot;hello&quot;</span><span class="keyword">,</span><span class="default">42</span><span class="keyword">); 
<br></span><span class="default">$ref&#xA0; &#xA0; </span><span class="keyword">= new </span><span class="default">ReflectionClass</span><span class="keyword">(</span><span class="string">&apos;mysqli_stmt&apos;</span><span class="keyword">); 
<br></span><span class="default">$method </span><span class="keyword">= </span><span class="default">$ref</span><span class="keyword">-&gt;</span><span class="default">getMethod</span><span class="keyword">(</span><span class="string">&quot;bind_param&quot;</span><span class="keyword">); 
<br></span><span class="default">$method</span><span class="keyword">-&gt;</span><span class="default">invokeArgs</span><span class="keyword">(</span><span class="default">$res</span><span class="keyword">,</span><span class="default">$refArr</span><span class="keyword">); 
<br></span><span class="default">$res</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">();&#xA0; 
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
A few notes on this function.<br><br>If you specify type &quot;i&quot; (integer), the maximum value it allows you to have is 2^32-1 or 2147483647. So, if you are using UNSIGNED INTEGER or BIGINT in your database, then you are better off using &quot;s&quot; (string) for this.<br><br>Here&apos;s a quick summary:<br>(UN)SIGNED TINYINT: I<br>(UN)SIGNED SMALLINT: I<br>(UN)SIGNED MEDIUMINT: I<br>SIGNED INT: I<br>UNSIGNED INT: S<br>(UN)SIGNED BIGINT: S<br><br>(VAR)CHAR, (TINY/SMALL/MEDIUM/BIG)TEXT/BLOB should all have S.<br><br>FLOAT/REAL/DOUBLE (PRECISION) should all be D.<br><br>That advice was for MySQL. I have not looked into other database software.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Hi, I just write a function to do all my sql statements based on all the others comments in this page, maybe it can be useful for someone else :)<br><br>Usage:<br><br>execSQL($sql, $parameters, $close);<br><br>$sql = Statement to execute;<br>$parameters = array of type and values of the parameters (if any)<br>$close = true to close $stmt (in inserts) false to return an array with the values;<br><br>Examples:<br><br>execSQL(&quot;SELECT * FROM table WHERE id = ?&quot;, array(&apos;i&apos;, $id), false);<br><br>execSQL(&quot;SELECT * FROM table&quot;, array(), false);<br><br>execSQL(&quot;INSERT INTO table(id, name) VALUES (?,?)&quot;, array(&apos;ss&apos;, $id, $name), true);<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">execSQL</span><span class="keyword">(</span><span class="default">$sql</span><span class="keyword">, </span><span class="default">$params</span><span class="keyword">, </span><span class="default">$close</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$mysqli </span><span class="keyword">= new </span><span class="default">mysqli</span><span class="keyword">(</span><span class="string">&quot;localhost&quot;</span><span class="keyword">, </span><span class="string">&quot;user&quot;</span><span class="keyword">, </span><span class="string">&quot;pass&quot;</span><span class="keyword">, </span><span class="string">&quot;db&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$stmt </span><span class="keyword">= </span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="default">$sql</span><span class="keyword">) or die (</span><span class="string">&quot;Failed to prepared the statement!&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">call_user_func_array</span><span class="keyword">(array(</span><span class="default">$stmt</span><span class="keyword">, </span><span class="string">&apos;bind_param&apos;</span><span class="keyword">), </span><span class="default">refValues</span><span class="keyword">(</span><span class="default">$params</span><span class="keyword">));<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; if(</span><span class="default">$close</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">affected_rows</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$meta </span><span class="keyword">= </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">result_metadata</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; while ( </span><span class="default">$field </span><span class="keyword">= </span><span class="default">$meta</span><span class="keyword">-&gt;</span><span class="default">fetch_field</span><span class="keyword">() ) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$parameters</span><span class="keyword">[] = &amp;</span><span class="default">$row</span><span class="keyword">[</span><span class="default">$field</span><span class="keyword">-&gt;</span><span class="default">name</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">call_user_func_array</span><span class="keyword">(array(</span><span class="default">$stmt</span><span class="keyword">, </span><span class="string">&apos;bind_result&apos;</span><span class="keyword">), </span><span class="default">refValues</span><span class="keyword">(</span><span class="default">$parameters</span><span class="keyword">));<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; while ( </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">fetch</span><span class="keyword">() ) {&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$x </span><span class="keyword">= array();&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; foreach( </span><span class="default">$row </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$val </span><span class="keyword">) {&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$x</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">$val</span><span class="keyword">;&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$results</span><span class="keyword">[] = </span><span class="default">$x</span><span class="keyword">;&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">$results</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">close</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">close</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return&#xA0; </span><span class="default">$result</span><span class="keyword">;<br>&#xA0;&#xA0; }<br>&#xA0;&#xA0; <br>&#xA0; &#xA0; function </span><span class="default">refValues</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">strnatcmp</span><span class="keyword">(</span><span class="default">phpversion</span><span class="keyword">(),</span><span class="string">&apos;5.3&apos;</span><span class="keyword">) &gt;= </span><span class="default">0</span><span class="keyword">) </span><span class="comment">//Reference is required for PHP 5.3+<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">{<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$refs </span><span class="keyword">= array();<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$arr </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$refs</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = &amp;</span><span class="default">$arr</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$refs</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$arr</span><span class="keyword">;<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;<br></span><br>Regards</span>
</div>
  

#


<div class="phpcode"><span class="html">
Blob and null handling aside, a couple of notes on how param values are automatically converted and forwarded on to the Mysql engine based on your type string argument:<br><br>1) PHP will automatically convert the value behind the scenes to the underlying type corresponding to your binding type string.&#xA0; i.e.:<br><br><span class="default">&lt;?php<br><br>$var </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;<br></span><span class="default">bind_param</span><span class="keyword">(</span><span class="string">&apos;i&apos;</span><span class="keyword">, </span><span class="default">$var</span><span class="keyword">); </span><span class="comment">// forwarded to Mysql as 1<br><br></span><span class="default">?&gt;<br></span><br>2) Though PHP numbers cannot be reliably cast to (int) if larger than PHP_INT_MAX, behind the scenes, the value will be converted anyway to at most long long depending on the size.&#xA0; This means that keeping in mind precision limits and avoiding manually casting the variable to (int) first, you can still use the &apos;i&apos; binding type for larger numbers.&#xA0; i.e.:<br><br><span class="default">&lt;?php<br><br>$var </span><span class="keyword">= </span><span class="string">&apos;429496729479896&apos;</span><span class="keyword">;<br></span><span class="default">bind_param</span><span class="keyword">(</span><span class="string">&apos;i&apos;</span><span class="keyword">, </span><span class="default">$var</span><span class="keyword">); </span><span class="comment">// forwarded to Mysql as 429496729479900<br><br></span><span class="default">?&gt;<br></span><br>3) You can default to &apos;s&apos; for most parameter arguments in most cases.&#xA0; The value will then be automatically cast to string on the back-end before being passed to the Mysql engine.&#xA0; Mysql will then perform its own conversions with values it receives from PHP on execute.&#xA0; This allows you to bind not only to larger numbers without concern for precision, but also to objects as long as that object has a &apos;__toString&apos; method.<br><br>This auto-string casting behavior greatly improves things like datetime handling.&#xA0; For example: if you extended DateTime class to add a __toString method which outputs the datetime format expected by Mysql, you can just bind to that DateTime_Extended object using type &apos;s&apos;.&#xA0; i.e.:<br><br><span class="default">&lt;?php<br><br></span><span class="comment">// DateTime_Extended has __toString defined to return the Mysql formatted datetime<br></span><span class="default">$var </span><span class="keyword">= new </span><span class="default">DateTime_Extended</span><span class="keyword">;<br></span><span class="default">bind_param</span><span class="keyword">(</span><span class="string">&apos;s&apos;</span><span class="keyword">, </span><span class="default">$var</span><span class="keyword">); </span><span class="comment">// forwarded to Mysql as &apos;2011-03-14 17:00:01&apos;<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Dear all,
<br>
<br>I was searching for a class which supports multiple calls to bind_param, because I have scenarios where I build huge SQL statements over different functions with variable numbers of parameters. But I didn&apos;t found one. So I have just written up this little piece of code I would like to share with you. There is enough room to optimize these classes, but it shows the general idea. And for me it works. In mbind_param_do() it seems to depend from the PHP version if makeValuesReferenced() must be used or if $params can be used directly. In my case I have to use it.
<br>
<br>The cool thing about this solution: You don&apos;t have to care about a lot if you are using my mbind_ functions or not. You may also use default bind_param and the execute will still work. 
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">class </span><span class="default">db </span><span class="keyword">extends </span><span class="default">mysqli </span><span class="keyword">{
<br>&#xA0; &#xA0; public function </span><span class="default">prepare</span><span class="keyword">(</span><span class="default">$query</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; return new </span><span class="default">stmt</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">,</span><span class="default">$query</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>}
<br>
<br>class </span><span class="default">stmt </span><span class="keyword">extends </span><span class="default">mysqli_stmt </span><span class="keyword">{
<br>&#xA0; &#xA0; public function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$link</span><span class="keyword">, </span><span class="default">$query</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">mbind_reset</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">parent</span><span class="keyword">::</span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$link</span><span class="keyword">, </span><span class="default">$query</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; public function </span><span class="default">mbind_reset</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; unset(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">mbind_params</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; unset(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">mbind_types</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">mbind_params </span><span class="keyword">= array();
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">mbind_types </span><span class="keyword">= array();
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="comment">//use this one to bind params by reference
<br>&#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">mbind_param</span><span class="keyword">(</span><span class="default">$type</span><span class="keyword">, &amp;</span><span class="default">$param</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">mbind_types</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">].= </span><span class="default">$type</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">mbind_params</span><span class="keyword">[] = &amp;</span><span class="default">$param</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="comment">//use this one to bin value directly, can be mixed with mbind_param()
<br>&#xA0; &#xA0; </span><span class="keyword">public function </span><span class="default">mbind_value</span><span class="keyword">(</span><span class="default">$type</span><span class="keyword">, </span><span class="default">$param</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">mbind_types</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">].= </span><span class="default">$type</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">mbind_params</span><span class="keyword">[] = </span><span class="default">$param</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; public function </span><span class="default">mbind_param_do</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$params </span><span class="keyword">= </span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">mbind_types</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">mbind_params</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">call_user_func_array</span><span class="keyword">(array(</span><span class="default">$this</span><span class="keyword">, </span><span class="string">&apos;bind_param&apos;</span><span class="keyword">), </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">makeValuesReferenced</span><span class="keyword">(</span><span class="default">$params</span><span class="keyword">));
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; private function </span><span class="default">makeValuesReferenced</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$refs </span><span class="keyword">= array();
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$arr </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$refs</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = &amp;</span><span class="default">$arr</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$refs</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; public function </span><span class="default">execute</span><span class="keyword">() {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">count</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">mbind_params</span><span class="keyword">))
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">mbind_param_do</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">parent</span><span class="keyword">::</span><span class="default">execute</span><span class="keyword">();
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; private </span><span class="default">$mbind_types </span><span class="keyword">= array();
<br>&#xA0; &#xA0; private </span><span class="default">$mbind_params </span><span class="keyword">= array();
<br>}
<br>
<br></span><span class="default">$search1 </span><span class="keyword">= </span><span class="string">&quot;test1&quot;</span><span class="keyword">;
<br></span><span class="default">$search2 </span><span class="keyword">= </span><span class="string">&quot;test2&quot;</span><span class="keyword">;
<br>
<br></span><span class="default">$_db </span><span class="keyword">= new </span><span class="default">db</span><span class="keyword">(</span><span class="string">&quot;host&quot;</span><span class="keyword">,</span><span class="string">&quot;user&quot;</span><span class="keyword">,</span><span class="string">&quot;pass&quot;</span><span class="keyword">,</span><span class="string">&quot;database&quot;</span><span class="keyword">);
<br></span><span class="default">$query </span><span class="keyword">= </span><span class="string">&quot;SELECT name FROM table WHERE col1=? AND col2=?&quot;</span><span class="keyword">;
<br></span><span class="default">$stmt </span><span class="keyword">= </span><span class="default">$_db</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="default">$query</span><span class="keyword">);
<br>
<br></span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">mbind_param</span><span class="keyword">(</span><span class="string">&apos;s&apos;</span><span class="keyword">,</span><span class="default">$search1</span><span class="keyword">);
<br></span><span class="comment">//this second call is the cool thing!!!
<br></span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">mbind_param</span><span class="keyword">(</span><span class="string">&apos;s&apos;</span><span class="keyword">,</span><span class="default">$search2</span><span class="keyword">);
<br>
<br></span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">();
<br>
<br></span><span class="comment">//this would still work!
<br>//$search1 = &quot;test1changed&quot;;
<br>//$search2 = &quot;test2changed&quot;;
<br>//$stmt-&gt;execute();
<br>
<br></span><span class="keyword">...
<br>
<br></span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">store_result</span><span class="keyword">();
<br></span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">bind_result</span><span class="keyword">(...);
<br></span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">fetch</span><span class="keyword">();
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I had a problem with the LIKE operator
<br>
<br>This code did not work:
<br>
<br><span class="default">&lt;?php
<br>$test </span><span class="keyword">= </span><span class="default">$sql</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="string">&quot;SELECT name FROM names WHERE name LIKE %?%&quot;</span><span class="keyword">);
<br></span><span class="default">$test</span><span class="keyword">-&gt;</span><span class="default">bind_param</span><span class="keyword">(</span><span class="string">&quot;s&quot;</span><span class="keyword">, </span><span class="default">$myname</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>The solution is:
<br>
<br><span class="default">&lt;?php
<br>$test </span><span class="keyword">= </span><span class="default">$sql</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="string">&quot;SELECT name FROM names WHERE name LIKE ?&quot;</span><span class="keyword">);
<br></span><span class="default">$param </span><span class="keyword">= </span><span class="string">&quot;%&quot; </span><span class="keyword">. </span><span class="default">$myname </span><span class="keyword">. </span><span class="string">&quot;%&quot;</span><span class="keyword">;
<br></span><span class="default">$test</span><span class="keyword">-&gt;</span><span class="default">bind_param</span><span class="keyword">(</span><span class="string">&quot;s&quot;</span><span class="keyword">, </span><span class="default">$param</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I used to have problems with call_user_func_array and bind_param after migrating to php 5.3.
<br>
<br>The problem is that 5.3 requires array values as reference while 5.2 works with real values.
<br>
<br>so i created a secondary function to help me with this...
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">refValues</span><span class="keyword">(</span><span class="default">$arr</span><span class="keyword">){
<br>&#xA0; &#xA0; if (</span><span class="default">strnatcmp</span><span class="keyword">(</span><span class="default">phpversion</span><span class="keyword">(),</span><span class="string">&apos;5.3&apos;</span><span class="keyword">) &gt;= </span><span class="default">0</span><span class="keyword">) </span><span class="comment">//Reference is required for PHP 5.3+
<br>&#xA0; &#xA0; </span><span class="keyword">{
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$refs </span><span class="keyword">= array();
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$arr </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$refs</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = &amp;</span><span class="default">$arr</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$refs</span><span class="keyword">; 
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return </span><span class="default">$arr</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>and changed my previous function from:
<br>
<br><span class="default">&lt;?php
<br>call_user_func_array</span><span class="keyword">(array(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">stmt</span><span class="keyword">, </span><span class="string">&quot;bind_param&quot;</span><span class="keyword">),</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">valores</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>to:
<br>
<br><span class="default">&lt;?php
<br>call_user_func_array</span><span class="keyword">(array(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">stmt</span><span class="keyword">, </span><span class="string">&quot;bind_param&quot;</span><span class="keyword">),</span><span class="default">refValues</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">valores</span><span class="keyword">));
<br></span><span class="default">?&gt;
<br></span>
<br>in this way my db functions keep working in php 5.2/5.3 servers.
<br>
<br>I hope this help someone.</span>
</div>
  

#


<div class="phpcode"><span class="html">
When dealing with a dynamic number of field values while preparing a statement I find this class useful.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">BindParam</span><span class="keyword">{
<br>&#xA0; &#xA0; private </span><span class="default">$values </span><span class="keyword">= array(), </span><span class="default">$types </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; public function </span><span class="default">add</span><span class="keyword">( </span><span class="default">$type</span><span class="keyword">, &amp;</span><span class="default">$value </span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">values</span><span class="keyword">[] = </span><span class="default">$value</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">types </span><span class="keyword">.= </span><span class="default">$type</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; public function </span><span class="default">get</span><span class="keyword">(){
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">array_merge</span><span class="keyword">(array(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">types</span><span class="keyword">), </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">values</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Usage is pretty simple. Create an instance and use the add method to populate. When you&apos;re ready to execute simply use the get method.
<br>
<br><span class="default">&lt;?php
<br>$bindParam </span><span class="keyword">= new </span><span class="default">BindParam</span><span class="keyword">();
<br></span><span class="default">$qArray </span><span class="keyword">= array();
<br>
<br></span><span class="default">$use_part_1 </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;
<br></span><span class="default">$use_part_2 </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;
<br></span><span class="default">$use_part_3 </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;
<br>
<br></span><span class="default">$query </span><span class="keyword">= </span><span class="string">&apos;SELECT * FROM users WHERE &apos;</span><span class="keyword">;
<br>if(</span><span class="default">$use_part_1</span><span class="keyword">){
<br>&#xA0; &#xA0; </span><span class="default">$qArray</span><span class="keyword">[] = </span><span class="string">&apos;hair_color = ?&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$bindParam</span><span class="keyword">-&gt;</span><span class="default">add</span><span class="keyword">(</span><span class="string">&apos;s&apos;</span><span class="keyword">, </span><span class="string">&apos;red&apos;</span><span class="keyword">);
<br>}
<br>if(</span><span class="default">$use_part_2</span><span class="keyword">){
<br>&#xA0; &#xA0; </span><span class="default">$qArray</span><span class="keyword">[] = </span><span class="string">&apos;age = ?&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$bindParam</span><span class="keyword">-&gt;</span><span class="default">add</span><span class="keyword">(</span><span class="string">&apos;i&apos;</span><span class="keyword">, </span><span class="default">25</span><span class="keyword">);
<br>}
<br>if(</span><span class="default">$use_part_3</span><span class="keyword">){
<br>&#xA0; &#xA0; </span><span class="default">$qArray</span><span class="keyword">[] = </span><span class="string">&apos;balance = ?&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$bindParam</span><span class="keyword">-&gt;</span><span class="default">add</span><span class="keyword">(</span><span class="string">&apos;d&apos;</span><span class="keyword">, </span><span class="default">50.00</span><span class="keyword">);
<br>}
<br>
<br></span><span class="default">$query </span><span class="keyword">.= </span><span class="default">implode</span><span class="keyword">(</span><span class="string">&apos; OR &apos;</span><span class="keyword">, </span><span class="default">$qArray</span><span class="keyword">);
<br>
<br></span><span class="comment">//call_user_func_array( array($stm, &apos;bind_param&apos;), $bindParam-&gt;get());
<br>
<br></span><span class="keyword">echo </span><span class="default">$query </span><span class="keyword">. </span><span class="string">&apos;&lt;br/&gt;&apos;</span><span class="keyword">;
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$bindParam</span><span class="keyword">-&gt;</span><span class="default">get</span><span class="keyword">());
<br></span><span class="default">?&gt;
<br></span>
<br>This gets you the result that looks something like this:
<br>
<br>SELECT * FROM users WHERE hair_color = ? OR age = ? OR balance = ?
<br>array(4) { [0]=&gt; string(3) &quot;sid&quot; [1]=&gt; string(3) &quot;red&quot; [2]=&gt; int(25) [3]=&gt; float(50) }
<br>
<br>[Editor&apos;s note: changed BindParam::add() to accept $value by reference and thereby prevent a warning in newer versions of PHP.]</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-stmt.bind-param.php)

**[To root](/README.md)**