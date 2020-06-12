# MongoCollection::remove




<div class="phpcode"><span class="html">
To remove a document based on its ID, you need to ensure that you pass the ID as a MongoID object rather than just a string:<br><br><span class="default">&lt;?php<br>$id </span><span class="keyword">= </span><span class="string">&apos;4b3f272c8ead0eb19d000000&apos;</span><span class="keyword">;<br><br></span><span class="comment">// will not work:<br></span><span class="default">$collection</span><span class="keyword">-&gt;</span><span class="default">remove</span><span class="keyword">(array(</span><span class="string">&apos;_id&apos; </span><span class="keyword">=&gt; </span><span class="default">$id</span><span class="keyword">), </span><span class="default">true</span><span class="keyword">);<br><br></span><span class="comment">// will work:<br></span><span class="default">$collection</span><span class="keyword">-&gt;</span><span class="default">remove</span><span class="keyword">(array(</span><span class="string">&apos;_id&apos; </span><span class="keyword">=&gt; new </span><span class="default">MongoId</span><span class="keyword">(</span><span class="default">$id</span><span class="keyword">)), </span><span class="default">true</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mongocollection.remove.php)

**[⬆ to root](/)**