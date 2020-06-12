# MongoCollection::update




<div class="phpcode"><span class="html">
For anyone referencing records by the Mongo _id object, it&apos;s important to recognise that it is in fact an object, and not a string. <br><br>If you have a record with a Mongo ID of say &quot;4e519d5118617e88f27ea8cd&quot; that you are trying to retrieve or update, you cannot search for it using something like:<br><span class="default">&lt;?php<br>$m </span><span class="keyword">= new </span><span class="default">Mongo</span><span class="keyword">();<br></span><span class="default">$db </span><span class="keyword">= </span><span class="default">$m</span><span class="keyword">-&gt;</span><span class="default">selectDB</span><span class="keyword">(</span><span class="string">&apos;db&apos;</span><span class="keyword">);<br></span><span class="default">$collection </span><span class="keyword">= </span><span class="string">&apos;collection&apos;</span><span class="keyword">;<br></span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">$collection</span><span class="keyword">-&gt;</span><span class="default">findOne</span><span class="keyword">(array(</span><span class="string">&apos;_id&apos;</span><span class="keyword">, </span><span class="string">&apos;4e519d5118617e88f27ea8cd&apos;</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>There is some documentation that mentions simple conversion to string will solve this, but I have found the only reliable way to locate records based on their ID is to first pass it to MondoID(), then use that for reference.<br><br>Something like this will be far more reliable:<br><span class="default">&lt;?php<br>$m </span><span class="keyword">= new </span><span class="default">Mongo</span><span class="keyword">();<br></span><span class="default">$db </span><span class="keyword">= </span><span class="default">$m</span><span class="keyword">-&gt;</span><span class="default">selectDB</span><span class="keyword">(</span><span class="string">&apos;db&apos;</span><span class="keyword">);<br></span><span class="default">$collection </span><span class="keyword">= </span><span class="string">&apos;collection&apos;</span><span class="keyword">;<br></span><span class="default">$mongoID </span><span class="keyword">= new </span><span class="default">MongoID</span><span class="keyword">(</span><span class="string">&apos;4e519d5118617e88f27ea8cd&apos;</span><span class="keyword">);<br></span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">$collection</span><span class="keyword">-&gt;</span><span class="default">findOne</span><span class="keyword">(array(</span><span class="string">&apos;_id&apos;</span><span class="keyword">, </span><span class="default">$mongoID</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>This may prove useful for anyone using the ID object like an auto-increment database key would be used in MySQL or similar.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Please note under optional third parameter &quot;options&quot;:<br><br>While the official MongoDB documentation references the keyword &quot;multi&quot; to flag the use of multiple updates, the PHP implementation uses the key &quot;multiple&quot; instead.<br><br>This may cause a little confusion if you&apos;re basing your keys on the OFFICIAL MongoDB documentation.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mongocollection.update.php)

**[⬆ to root](/)**