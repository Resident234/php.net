# $HTTP_RAW_POST_DATA




<div class="phpcode"><span class="html">
To get the Raw Post Data:
<br>
<br><span class="default">&lt;?php $postdata </span><span class="keyword">= </span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="string">&quot;php://input&quot;</span><span class="keyword">); </span><span class="default">?&gt;
<br></span>
<br>Please see the notes here:
<br><a href="http://us.php.net/manual/en/wrappers.php.php" rel="nofollow" target="_blank">http://us.php.net/manual/en/wrappers.php.php</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
what is exaclty raw POST data?<br><br>Answer:<br><br>$_POST can be said as and outcome after splitting the $HTTP_RAW_POST_DATA, php splits the raw post data and formats in the way we see it in the $_POST For example:<br><br>&#xA0; &#xA0; $HTTP_RAW_POST_DATA looks something like this<br><br>key1=value1&amp;key2=value2<br><br>&#xA0; &#xA0; then $_POST would look like this:<br><br>$_POST = array(<br>&#xA0; &#xA0; &quot;key1&quot; =&gt; &quot;value1&quot;,<br>&#xA0; &#xA0; &quot;key2&quot; =&gt; &quot;value2&quot;,);</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.httprawpostdata.php)

**[To root](/)**