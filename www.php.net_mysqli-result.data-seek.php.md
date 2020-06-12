# mysqli_result::data_seek




<div class="phpcode"><span class="html">
This is useful function when you try to loop through the resultset numerous times.<br><br>For example:<br><br><span class="default">&lt;?php<br><br>$result </span><span class="keyword">= </span><span class="default">mysqli_query</span><span class="keyword">(</span><span class="default">$connection_id</span><span class="keyword">,</span><span class="default">$query</span><span class="keyword">);<br><br>while (</span><span class="default">$row </span><span class="keyword">= </span><span class="default">mysqli_fetch_assoc</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">))<br> {<br>&#xA0; </span><span class="comment">// Looping through the resultset.<br> </span><span class="keyword">}<br><br></span><span class="comment">// Now if you need to loop through it again, you would first call the seek function:<br></span><span class="default">mysqli_data_seek</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">);<br><br>while (</span><span class="default">$row </span><span class="keyword">= </span><span class="default">mysqli_fetch_assoc</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">))<br> {<br>&#xA0; </span><span class="comment">// Looping through the resultset again.<br> </span><span class="keyword">}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-result.data-seek.php)

**[⬆ to root](/)**