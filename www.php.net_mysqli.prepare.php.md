# mysqli::prepare




<div class="phpcode"><span class="html">
Just wanted to make sure that all were aware of get_result.<br><br>In the code sample, after execute(), perform a get_result() like this:<br><br><span class="default">&lt;?php<br><br></span><span class="comment">// ... document&apos;s example code:<br><br>&#xA0; &#xA0; /* bind parameters for markers */<br>&#xA0; &#xA0; </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">bind_param</span><span class="keyword">(</span><span class="string">&quot;s&quot;</span><span class="keyword">, </span><span class="default">$city</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="comment">/* execute query */<br>&#xA0; &#xA0; </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">();<br><br>&#xA0; &#xA0; </span><span class="comment">/* instead of bind_result: */<br>&#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">get_result</span><span class="keyword">();<br><br>&#xA0; &#xA0; </span><span class="comment">/* now you can fetch the results into an array - NICE */<br>&#xA0; &#xA0; </span><span class="keyword">while (</span><span class="default">$myrow </span><span class="keyword">= </span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">fetch_assoc</span><span class="keyword">()) {<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// use your $myrow array as you would with any other fetch<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">printf</span><span class="keyword">(</span><span class="string">&quot;%s is in district %s\n&quot;</span><span class="keyword">, </span><span class="default">$city</span><span class="keyword">, </span><span class="default">$myrow</span><span class="keyword">[</span><span class="string">&apos;district&apos;</span><span class="keyword">]);<br><br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;<br></span><br>This is much nicer when you have a dozen or more fields coming back from your query.&#xA0; Hope this helps.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I wrote this function for my personal use and figured I would share it.&#xA0; I am not sure if this is the appropriate forum but I wish I had this when I stumbled on to mysqli::prepare.&#xA0; The function is an update of the function I posted previously.&#xA0; The previous function could not handle multiple queries.
<br>
<br>For queries:
<br>Results of single queries are given as arrays[row#][associated Data Array]
<br>Results of multiple queries are given as arrays[query#][row#][associated Data Array]
<br>
<br>For queries which return an affected row#, affected rows are returned instead of (array[row#][associated Data Array])
<br>
<br>Code and example are below:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">mysqli_prepared_query</span><span class="keyword">(</span><span class="default">$link</span><span class="keyword">,</span><span class="default">$sql</span><span class="keyword">,</span><span class="default">$typeDef </span><span class="keyword">= </span><span class="default">FALSE</span><span class="keyword">,</span><span class="default">$params </span><span class="keyword">= </span><span class="default">FALSE</span><span class="keyword">){
<br>&#xA0; if(</span><span class="default">$stmt </span><span class="keyword">= </span><span class="default">mysqli_prepare</span><span class="keyword">(</span><span class="default">$link</span><span class="keyword">,</span><span class="default">$sql</span><span class="keyword">)){
<br>&#xA0; &#xA0; if(</span><span class="default">count</span><span class="keyword">(</span><span class="default">$params</span><span class="keyword">) == </span><span class="default">count</span><span class="keyword">(</span><span class="default">$params</span><span class="keyword">,</span><span class="default">1</span><span class="keyword">)){
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$params </span><span class="keyword">= array(</span><span class="default">$params</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$multiQuery </span><span class="keyword">= </span><span class="default">FALSE</span><span class="keyword">;
<br>&#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$multiQuery </span><span class="keyword">= </span><span class="default">TRUE</span><span class="keyword">;
<br>&#xA0; &#xA0; }&#xA0; 
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; if(</span><span class="default">$typeDef</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$bindParams </span><span class="keyword">= array();&#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$bindParamsReferences </span><span class="keyword">= array();
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$bindParams </span><span class="keyword">= </span><span class="default">array_pad</span><span class="keyword">(</span><span class="default">$bindParams</span><span class="keyword">,(</span><span class="default">count</span><span class="keyword">(</span><span class="default">$params</span><span class="keyword">,</span><span class="default">1</span><span class="keyword">)-</span><span class="default">count</span><span class="keyword">(</span><span class="default">$params</span><span class="keyword">))/</span><span class="default">count</span><span class="keyword">(</span><span class="default">$params</span><span class="keyword">),</span><span class="string">&quot;&quot;</span><span class="keyword">);&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
<br>&#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$bindParams </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$bindParamsReferences</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = &amp;</span><span class="default">$bindParams</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">];&#xA0; 
<br>&#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">array_unshift</span><span class="keyword">(</span><span class="default">$bindParamsReferences</span><span class="keyword">,</span><span class="default">$typeDef</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$bindParamsMethod </span><span class="keyword">= new </span><span class="default">ReflectionMethod</span><span class="keyword">(</span><span class="string">&apos;mysqli_stmt&apos;</span><span class="keyword">, </span><span class="string">&apos;bind_param&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$bindParamsMethod</span><span class="keyword">-&gt;</span><span class="default">invokeArgs</span><span class="keyword">(</span><span class="default">$stmt</span><span class="keyword">,</span><span class="default">$bindParamsReferences</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= array();
<br>&#xA0; &#xA0; foreach(</span><span class="default">$params </span><span class="keyword">as </span><span class="default">$queryKey </span><span class="keyword">=&gt; </span><span class="default">$query</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$bindParams </span><span class="keyword">as </span><span class="default">$paramKey </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$bindParams</span><span class="keyword">[</span><span class="default">$paramKey</span><span class="keyword">] = </span><span class="default">$query</span><span class="keyword">[</span><span class="default">$paramKey</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$queryResult </span><span class="keyword">= array();
<br>&#xA0; &#xA0; &#xA0; if(</span><span class="default">mysqli_stmt_execute</span><span class="keyword">(</span><span class="default">$stmt</span><span class="keyword">)){
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$resultMetaData </span><span class="keyword">= </span><span class="default">mysqli_stmt_result_metadata</span><span class="keyword">(</span><span class="default">$stmt</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$resultMetaData</span><span class="keyword">){&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$stmtRow </span><span class="keyword">= array();&#xA0;&#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$rowReferences </span><span class="keyword">= array(); 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; while (</span><span class="default">$field </span><span class="keyword">= </span><span class="default">mysqli_fetch_field</span><span class="keyword">(</span><span class="default">$resultMetaData</span><span class="keyword">)) { 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$rowReferences</span><span class="keyword">[] = &amp;</span><span class="default">$stmtRow</span><span class="keyword">[</span><span class="default">$field</span><span class="keyword">-&gt;</span><span class="default">name</span><span class="keyword">]; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">mysqli_free_result</span><span class="keyword">(</span><span class="default">$resultMetaData</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$bindResultMethod </span><span class="keyword">= new </span><span class="default">ReflectionMethod</span><span class="keyword">(</span><span class="string">&apos;mysqli_stmt&apos;</span><span class="keyword">, </span><span class="string">&apos;bind_result&apos;</span><span class="keyword">); 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$bindResultMethod</span><span class="keyword">-&gt;</span><span class="default">invokeArgs</span><span class="keyword">(</span><span class="default">$stmt</span><span class="keyword">, </span><span class="default">$rowReferences</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; while(</span><span class="default">mysqli_stmt_fetch</span><span class="keyword">(</span><span class="default">$stmt</span><span class="keyword">)){
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$row </span><span class="keyword">= array();
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$stmtRow </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$row</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">$value</span><span class="keyword">;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$queryResult</span><span class="keyword">[] = </span><span class="default">$row</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">mysqli_stmt_free_result</span><span class="keyword">(</span><span class="default">$stmt</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$queryResult</span><span class="keyword">[] = </span><span class="default">mysqli_stmt_affected_rows</span><span class="keyword">(</span><span class="default">$stmt</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$queryResult</span><span class="keyword">[] = </span><span class="default">FALSE</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; } 
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$result</span><span class="keyword">[</span><span class="default">$queryKey</span><span class="keyword">] = </span><span class="default">$queryResult</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; </span><span class="default">mysqli_stmt_close</span><span class="keyword">(</span><span class="default">$stmt</span><span class="keyword">);&#xA0;&#xA0; 
<br>&#xA0; } else {
<br>&#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">FALSE</span><span class="keyword">;
<br>&#xA0; }
<br>&#xA0; 
<br>&#xA0; if(</span><span class="default">$multiQuery</span><span class="keyword">){
<br>&#xA0; &#xA0; return </span><span class="default">$result</span><span class="keyword">;
<br>&#xA0; } else {
<br>&#xA0; &#xA0; return </span><span class="default">$result</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">];
<br>&#xA0; }
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Example(s):
<br>For a table of firstName and lastName:
<br>John Smith
<br>Mark Smith
<br>Jack Johnson
<br>Bob Johnson
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">//single query, single result
<br></span><span class="default">$query </span><span class="keyword">= </span><span class="string">&quot;SELECT * FROM names WHERE firstName=? AND lastName=?&quot;</span><span class="keyword">;
<br></span><span class="default">$params </span><span class="keyword">= array(</span><span class="string">&quot;Bob&quot;</span><span class="keyword">,</span><span class="string">&quot;Johnson&quot;</span><span class="keyword">);
<br>
<br></span><span class="default">mysqli_prepared_query</span><span class="keyword">(</span><span class="default">$link</span><span class="keyword">,</span><span class="default">$query</span><span class="keyword">,</span><span class="string">&quot;ss&quot;</span><span class="keyword">,</span><span class="default">$params</span><span class="keyword">)
<br></span><span class="comment">/*
<br>returns array(
<br>0=&gt; array(&apos;firstName&apos; =&gt; &apos;Bob&apos;, &apos;lastName&apos; =&gt; &apos;Johnson&apos;)
<br>)
<br>*/
<br>
<br>//single query, multiple results
<br></span><span class="default">$query </span><span class="keyword">= </span><span class="string">&quot;SELECT * FROM names WHERE lastName=?&quot;</span><span class="keyword">;
<br></span><span class="default">$params </span><span class="keyword">= array(</span><span class="string">&quot;Smith&quot;</span><span class="keyword">);
<br>
<br></span><span class="default">mysqli_prepared_query</span><span class="keyword">(</span><span class="default">$link</span><span class="keyword">,</span><span class="default">$query</span><span class="keyword">,</span><span class="string">&quot;s&quot;</span><span class="keyword">,</span><span class="default">$params</span><span class="keyword">)
<br></span><span class="comment">/*
<br>returns array(
<br>0=&gt; array(&apos;firstName&apos; =&gt; &apos;John&apos;, &apos;lastName&apos; =&gt; &apos;Smith&apos;)
<br>1=&gt; array(&apos;firstName&apos; =&gt; &apos;Mark&apos;, &apos;lastName&apos; =&gt; &apos;Smith&apos;)
<br>)
<br>*/
<br>
<br>//multiple query, multiple results
<br></span><span class="default">$query </span><span class="keyword">= </span><span class="string">&quot;SELECT * FROM names WHERE lastName=?&quot;</span><span class="keyword">;
<br></span><span class="default">$params </span><span class="keyword">= array(array(</span><span class="string">&quot;Smith&quot;</span><span class="keyword">),array(</span><span class="string">&quot;Johnson&quot;</span><span class="keyword">));
<br>
<br></span><span class="default">mysqli_prepared_query</span><span class="keyword">(</span><span class="default">$link</span><span class="keyword">,</span><span class="default">$query</span><span class="keyword">,</span><span class="string">&quot;s&quot;</span><span class="keyword">,</span><span class="default">$params</span><span class="keyword">)
<br></span><span class="comment">/*
<br>returns array(
<br>0=&gt;
<br>array(
<br>0=&gt; array(&apos;firstName&apos; =&gt; &apos;John&apos;, &apos;lastName&apos; =&gt; &apos;Smith&apos;)
<br>1=&gt; array(&apos;firstName&apos; =&gt; &apos;Mark&apos;, &apos;lastName&apos; =&gt; &apos;Smith&apos;)
<br>)
<br>1=&gt;
<br>array(
<br>0=&gt; array(&apos;firstName&apos; =&gt; &apos;Jack&apos;, &apos;lastName&apos; =&gt; &apos;Johnson&apos;)
<br>1=&gt; array(&apos;firstName&apos; =&gt; &apos;Bob&apos;, &apos;lastName&apos; =&gt; &apos;Johnson&apos;)
<br>)
<br>)
<br>*/
<br></span><span class="default">?&gt;
<br></span>
<br>Hope it helps =)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.prepare.php)

**[⬆ to root](/)**