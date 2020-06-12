# mysqli_result::fetch_assoc




<div class="phpcode"><span class="html">
I often like to have my results sent elsewhere in the format of an array (although keep in mind that if you just plan on traversing through the array in another part of the script, this extra step is just a waste of time).<br><br>This is my one-liner for transforming a mysqli_result set into an array.<br><span class="default">&lt;?php<br>$sql </span><span class="keyword">= new </span><span class="default">MySQLi</span><span class="keyword">(</span><span class="default">$host</span><span class="keyword">, </span><span class="default">$username</span><span class="keyword">, </span><span class="default">$password</span><span class="keyword">, </span><span class="default">$database</span><span class="keyword">);<br><br></span><span class="default">$result </span><span class="keyword">= </span><span class="default">$sql</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;SELECT * FROM `</span><span class="default">$table</span><span class="string">`;&quot;</span><span class="keyword">);<br>for (</span><span class="default">$set </span><span class="keyword">= array (); </span><span class="default">$row </span><span class="keyword">= </span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">fetch_assoc</span><span class="keyword">(); </span><span class="default">$set</span><span class="keyword">[] = </span><span class="default">$row</span><span class="keyword">);<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$set</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Outputs:<br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [id] =&gt; 1<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [field2] =&gt; a<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [field3] =&gt; b<br>&#xA0; &#xA0; &#xA0; &#xA0; ),<br>&#xA0; &#xA0; [1] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [id] =&gt; 2<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [field2] =&gt; c<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [field3] =&gt; d<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br>)<br><br>I use other variations to adapt to the situation, i.e. if I am selecting only one field:<br><span class="default">&lt;?php<br>$sql </span><span class="keyword">= new </span><span class="default">MySQLi</span><span class="keyword">(</span><span class="default">$host</span><span class="keyword">, </span><span class="default">$username</span><span class="keyword">, </span><span class="default">$password</span><span class="keyword">, </span><span class="default">$database</span><span class="keyword">);<br></span><span class="default">$result </span><span class="keyword">= </span><span class="default">$sql</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;SELECT `field2` FROM `</span><span class="default">$table</span><span class="string">`;&quot;</span><span class="keyword">);<br>for (</span><span class="default">$set </span><span class="keyword">= array (); </span><span class="default">$row </span><span class="keyword">= </span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">fetch_assoc</span><span class="keyword">(); </span><span class="default">$set</span><span class="keyword">[] = </span><span class="default">$row</span><span class="keyword">[</span><span class="string">&apos;field2&apos;</span><span class="keyword">]);<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$set</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span>Outputs:<br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; a<br>&#xA0; &#xA0; [1] =&gt; c<br>)<br><br>Or, to make the array associative with the primary index (code assumes primary index is the first field in the table):<br><span class="default">&lt;?php<br>$sql </span><span class="keyword">= new </span><span class="default">MySQLi</span><span class="keyword">(</span><span class="default">$host</span><span class="keyword">, </span><span class="default">$username</span><span class="keyword">, </span><span class="default">$password</span><span class="keyword">, </span><span class="default">$database</span><span class="keyword">);<br></span><span class="default">$result </span><span class="keyword">= </span><span class="default">$sql</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;SELECT * FROM `</span><span class="default">$table</span><span class="string">`;&quot;</span><span class="keyword">);<br>for (</span><span class="default">$set </span><span class="keyword">= array (); </span><span class="default">$row </span><span class="keyword">= </span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">fetch_assoc</span><span class="keyword">(); </span><span class="default">$set</span><span class="keyword">[</span><span class="default">array_shift</span><span class="keyword">(</span><span class="default">$row</span><span class="keyword">)] = </span><span class="default">$row</span><span class="keyword">);<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$set</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span>Outputs:<br>Array<br>(<br>&#xA0; &#xA0; [1] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [field2] =&gt; a<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [field3] =&gt; b<br>&#xA0; &#xA0; &#xA0; &#xA0; ),<br>&#xA0; &#xA0; [2] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [field2] =&gt; c<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [field3] =&gt; d<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
IMPORTANT NOTE:
<br>
<br>If you were used to using code like this:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">while(</span><span class="default">false </span><span class="keyword">!== (</span><span class="default">$row </span><span class="keyword">= </span><span class="default">mysql_fetch_assoc</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">)))
<br>{
<br>&#xA0; &#xA0; </span><span class="comment">//...
<br></span><span class="keyword">}
<br></span><span class="default">?&gt;
<br></span>
<br>You must change it to this for mysqli:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">while(</span><span class="default">null </span><span class="keyword">!== (</span><span class="default">$row </span><span class="keyword">= </span><span class="default">mysqli_fetch_assoc</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">)))
<br>{
<br>&#xA0; &#xA0; </span><span class="comment">//...
<br></span><span class="keyword">}
<br></span><span class="default">?&gt;
<br></span>
<br>The former will cause your script to run until max_execution_time is reached.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-result.fetch-assoc.php)

**[⬆ to root](/)**