# mysqli_stmt::bind_resultmysqli_stmt_bind_result




<div class="phpcode"><span class="html">
lot of people don&apos;t like how bind_result works with prepared statements! it requires you to pass long list of parameters which will be loaded with column value when the function being called.
<br>
<br>To solve this, i used call_user_func_array function and result_metadata functions. which make easy and automatically returns an array of all columns results stored in an array with column names.
<br>
<br>please don&apos;t forget to change setting variables with your own credentials:
<br>
<br><span class="default">&lt;?php
<br>$host </span><span class="keyword">= </span><span class="string">&apos;localhost&apos;</span><span class="keyword">;
<br></span><span class="default">$user </span><span class="keyword">= </span><span class="string">&apos;root&apos;</span><span class="keyword">;
<br></span><span class="default">$pass </span><span class="keyword">= </span><span class="string">&apos;1234&apos;</span><span class="keyword">;
<br></span><span class="default">$data </span><span class="keyword">= </span><span class="string">&apos;test&apos;</span><span class="keyword">;
<br>
<br></span><span class="default">$mysqli </span><span class="keyword">= new </span><span class="default">mysqli</span><span class="keyword">(</span><span class="default">$host</span><span class="keyword">, </span><span class="default">$user</span><span class="keyword">, </span><span class="default">$pass</span><span class="keyword">, </span><span class="default">$data</span><span class="keyword">);
<br></span><span class="comment">/* check connection */
<br></span><span class="keyword">if (</span><span class="default">mysqli_connect_errno</span><span class="keyword">()) {
<br>&#xA0; &#xA0; </span><span class="default">printf</span><span class="keyword">(</span><span class="string">&quot;Connect failed: %s\n&quot;</span><span class="keyword">, </span><span class="default">mysqli_connect_error</span><span class="keyword">());
<br>&#xA0; &#xA0; exit();
<br>}
<br>
<br>if (</span><span class="default">$stmt </span><span class="keyword">= </span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="string">&quot;SELECT * FROM sample WHERE t2 LIKE ?&quot;</span><span class="keyword">)) {
<br>&#xA0; &#xA0; </span><span class="default">$tt2 </span><span class="keyword">= </span><span class="string">&apos;%&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">bind_param</span><span class="keyword">(</span><span class="string">&quot;s&quot;</span><span class="keyword">, </span><span class="default">$tt2</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">();
<br>
<br>&#xA0; &#xA0; </span><span class="default">$meta </span><span class="keyword">= </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">result_metadata</span><span class="keyword">();
<br>&#xA0; &#xA0; while (</span><span class="default">$field </span><span class="keyword">= </span><span class="default">$meta</span><span class="keyword">-&gt;</span><span class="default">fetch_field</span><span class="keyword">())
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$params</span><span class="keyword">[] = &amp;</span><span class="default">$row</span><span class="keyword">[</span><span class="default">$field</span><span class="keyword">-&gt;</span><span class="default">name</span><span class="keyword">];
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; </span><span class="default">call_user_func_array</span><span class="keyword">(array(</span><span class="default">$stmt</span><span class="keyword">, </span><span class="string">&apos;bind_result&apos;</span><span class="keyword">), </span><span class="default">$params</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; while (</span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">fetch</span><span class="keyword">()) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$row </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$val</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$c</span><span class="keyword">[</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">$val</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result</span><span class="keyword">[] = </span><span class="default">$c</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">close</span><span class="keyword">();
<br>}
<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">close</span><span class="keyword">();
<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I wrote a function that fetches all rows from a result set - either normal or prepared.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">fetch</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">)<br>{&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">$array </span><span class="keyword">= array();<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; if(</span><span class="default">$result </span><span class="keyword">instanceof </span><span class="default">mysqli_stmt</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">store_result</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$variables </span><span class="keyword">= array();<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$data </span><span class="keyword">= array();<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$meta </span><span class="keyword">= </span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">result_metadata</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; while(</span><span class="default">$field </span><span class="keyword">= </span><span class="default">$meta</span><span class="keyword">-&gt;</span><span class="default">fetch_field</span><span class="keyword">())<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$variables</span><span class="keyword">[] = &amp;</span><span class="default">$data</span><span class="keyword">[</span><span class="default">$field</span><span class="keyword">-&gt;</span><span class="default">name</span><span class="keyword">]; </span><span class="comment">// pass by reference<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">call_user_func_array</span><span class="keyword">(array(</span><span class="default">$result</span><span class="keyword">, </span><span class="string">&apos;bind_result&apos;</span><span class="keyword">), </span><span class="default">$variables</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$i</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; while(</span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">fetch</span><span class="keyword">())<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$array</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">] = array();<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$data </span><span class="keyword">as </span><span class="default">$k</span><span class="keyword">=&gt;</span><span class="default">$v</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$array</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">][</span><span class="default">$k</span><span class="keyword">] = </span><span class="default">$v</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$i</span><span class="keyword">++;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// don&apos;t know why, but when I tried $array[] = $data, I got the same one result in all rows<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; elseif(</span><span class="default">$result </span><span class="keyword">instanceof </span><span class="default">mysqli_result</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; while(</span><span class="default">$row </span><span class="keyword">= </span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">fetch_assoc</span><span class="keyword">())<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$array</span><span class="keyword">[] = </span><span class="default">$row</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; return </span><span class="default">$array</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>Simply call it passing a result set or executed statement and you&apos;ll get all rows fetched.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you select LOBs use the following order of execution or you risk mysqli allocating more memory that actually used
<br>
<br>1)prepare()
<br>2)execute()
<br>3)store_result()
<br>4)bind_result()
<br>
<br>If you skip 3) or exchange 3) and 4) then mysqli will allocate memory for the maximal length of the column which is 255 for tinyblob, 64k for blob(still ok), 16MByte for MEDIUMBLOB - quite a lot and 4G for LONGBLOB (good if you have so much memory). Queries which use this order a bit slower when there is a LOB but this is the price of not having memory exhaustion in seconds.</span>
</div>
  

#


<div class="phpcode"><span class="html">
A note to people to want to return an array of results - that is, an array of all the results from the query, not just one at a time.<br><br><span class="default">&lt;?php<br><br></span><span class="comment">// blah blah...<br></span><span class="default">call_user_func_array</span><span class="keyword">(array(</span><span class="default">$mysqli_stmt_object</span><span class="keyword">, </span><span class="string">&quot;bind_result&quot;</span><span class="keyword">), </span><span class="default">$byref_array_for_fields</span><span class="keyword">);<br><br></span><span class="default">$results </span><span class="keyword">= array();<br>while (</span><span class="default">$mysqli_stmt_object</span><span class="keyword">-&gt;</span><span class="default">fetch</span><span class="keyword">()) {<br>&#xA0; &#xA0; </span><span class="default">$results</span><span class="keyword">[] = </span><span class="default">$byref_array_for_fields</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;<br></span>This will NOT work. $results will have a bunch of arrays, but each one will have a reference to $byref.<br><br>PHP is optimizing performance here: you aren&apos;t so much copying the $byref array into $results as you are *adding* it. That means $results will have a bunch of $byrefs - the same array repeated multiple times. (So what you see is that $results is all duplicates of the last item from the query.)<br><br>hamidhossain (01-Sep-2008) shows how to get around that: inside the loop that fetches results you also have to loop through the list of fields, copying them as you go. In effect, copying everything individually.<br><br>Personally, I&apos;d rather use some kind of function that effectively duplicates an array than write my own code. Many of the built-in array functions don&apos;t work, apparently using references rather than copies, but a combination of array_map and create_function does.<br><br><span class="default">&lt;?php<br><br></span><span class="comment">// blah blah...<br></span><span class="default">call_user_func_array</span><span class="keyword">(array(</span><span class="default">$mysqli_stmt_object</span><span class="keyword">, </span><span class="string">&quot;bind_result&quot;</span><span class="keyword">), </span><span class="default">$byref_array_for_fields</span><span class="keyword">);<br><br></span><span class="comment">// returns a copy of a value<br></span><span class="default">$copy </span><span class="keyword">= </span><span class="default">create_function</span><span class="keyword">(</span><span class="string">&apos;$a&apos;</span><span class="keyword">, </span><span class="string">&apos;return $a;&apos;</span><span class="keyword">);<br><br></span><span class="default">$results </span><span class="keyword">= array();<br>while (</span><span class="default">$mysqli_stmt_object</span><span class="keyword">-&gt;</span><span class="default">fetch</span><span class="keyword">()) {<br>&#xA0; &#xA0; </span><span class="comment">// array_map will preserve keys when done here and this way<br>&#xA0; &#xA0; </span><span class="default">$results</span><span class="keyword">[] = </span><span class="default">array_map</span><span class="keyword">(</span><span class="default">$copy</span><span class="keyword">, </span><span class="default">$byref_array_for_fields</span><span class="keyword">);<br>}<br><br></span><span class="default">?&gt;<br></span><br>All these problems would go away if they just implemented a fetch_assoc or even fetch_array for prepared statements...</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-stmt.bind-result.php)

**[To root](/README.md)**