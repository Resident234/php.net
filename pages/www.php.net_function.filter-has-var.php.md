# filter_has_var




<div class="phpcode"><span class="html">
Please note that the function does not check the live array, it actually checks the content received by php:<br><br><span class="default">&lt;?php<br>$_GET</span><span class="keyword">[</span><span class="string">&apos;test&apos;</span><span class="keyword">] = </span><span class="default">1</span><span class="keyword">;<br>echo </span><span class="default">filter_has_var</span><span class="keyword">(</span><span class="default">INPUT_GET</span><span class="keyword">, </span><span class="string">&apos;test&apos;</span><span class="keyword">) ? </span><span class="string">&apos;Yes&apos; </span><span class="keyword">: </span><span class="string">&apos;No&apos;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>would say &quot;No&quot;, unless the parameter was actually in the querystring.<br><br>Also, if the input var is empty, it will say Yes.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Through this example i think you can better understand<br><br>&#xA0; &#xA0; if ( !filter_has_var(INPUT_GET, &apos;email&apos;) ) {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;Email Not Found&quot;;<br>&#xA0; &#xA0; }else{<br>&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;Email Found&quot;;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; Output<br><br>&#xA0; &#xA0; localhost/nanhe/test.php?email=1 //Email Found<br>&#xA0; &#xA0; localhost/nanhe/test.php?email //Email Found<br>&#xA0; &#xA0; <a href="http://localhost/nanhe/test.php" rel="nofollow" target="_blank">http://localhost/nanhe/test.php</a> //Email Not Found<br><br>Consider on second example<br><br><a href="http://localhost/nanhe/test.php" rel="nofollow" target="_blank">http://localhost/nanhe/test.php</a><br>$_GET[&apos;email&apos;]=&quot;info@nanhe.in&quot;;<br>if ( !filter_has_var(INPUT_GET, &apos;email&apos;) ) {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;Email Not Found&quot;;<br>&#xA0; &#xA0; }else{<br>&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;Email Found&quot;;<br>&#xA0; &#xA0; }<br>But output will be Email Not Found</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.filter-has-var.php)

**[To root](/README.md)**