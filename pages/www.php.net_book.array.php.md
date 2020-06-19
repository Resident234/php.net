# Arrays




<div class="phpcode"><span class="html">
For newbies like me.<br><br>Creating new arrays:-<br>//Creates a blank array.<br>$theVariable = array();<br><br>//Creates an array with elements.<br>$theVariable = array(&quot;A&quot;, &quot;B&quot;, &quot;C&quot;);<br><br>//Creating Associaive array.<br>$theVariable = array(1 =&gt; &quot;http//google.com&quot;, 2=&gt; &quot;<a href="http://yahoo.com" rel="nofollow" target="_blank">http://yahoo.com</a>&quot;);<br><br>//Creating Associaive array with named keys<br>$theVariable = array(&quot;google&quot; =&gt; &quot;http//google.com&quot;, &quot;yahoo&quot;=&gt; &quot;<a href="http://yahoo.com" rel="nofollow" target="_blank">http://yahoo.com</a>&quot;);<br><br>Note:<br>New value can be added to the array as shown below.<br>$theVariable[] = &quot;D&quot;;<br>$theVariable[] = &quot;E&quot;;</span>
</div>
  

#


<div class="phpcode"><span class="html">
To delete an individual array element use the unset function<br><br>For example:<br><br><span class="default">&lt;?PHP<br>&#xA0; &#xA0; $arr </span><span class="keyword">= array( </span><span class="string">&quot;A&quot;</span><span class="keyword">, </span><span class="string">&quot;B&quot;</span><span class="keyword">, </span><span class="string">&quot;C&quot; </span><span class="keyword">);<br>&#xA0; &#xA0; unset( </span><span class="default">$arr</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] );<br>&#xA0; &#xA0; </span><span class="comment">// now $arr = array( &quot;A&quot;, &quot;C&quot; );<br></span><span class="default">?&gt;<br></span><br>Unlink is for deleting files.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/book.array.php)

**[To root](/README.md)**