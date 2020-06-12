# MongoId::isValid




<div class="phpcode"><span class="html">
There is no equivalent for this method in the new extension, so instead use&#x2026;<br><br><span class="default">&lt;?php<br></span><span class="keyword">if (</span><span class="default">$id </span><span class="keyword">instanceof \</span><span class="default">MongoDB</span><span class="keyword">\</span><span class="default">BSON</span><span class="keyword">\</span><span class="default">ObjectID<br>&#xA0; &#xA0; </span><span class="keyword">|| </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/^[a-f\d]{24}$/i&apos;</span><span class="keyword">, </span><span class="default">$id</span><span class="keyword">)<br>) {<br>&#xA0; </span><span class="default">&#x2026;<br></span><span class="keyword">}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mongoid.isvalid.php)

**[⬆ to root](/)**