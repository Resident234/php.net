# Using the PHP Library for MongoDB (PHPLIB)




<div class="phpcode"><span class="html">
To test your connection string, you can do something like this:<br><br><span class="default">&lt;?php<br>$mongo </span><span class="keyword">= new </span><span class="default">MongoDB</span><span class="keyword">\</span><span class="default">Client</span><span class="keyword">(</span><span class="string">&apos;mongodb://my_server_does_not_exist_here:27017&apos;</span><span class="keyword">);<br>try <br>{<br>&#xA0; &#xA0; </span><span class="default">$dbs </span><span class="keyword">= </span><span class="default">$mongo</span><span class="keyword">-&gt;</span><span class="default">listDatabases</span><span class="keyword">();<br>}<br>catch (</span><span class="default">MongoDB</span><span class="keyword">\</span><span class="default">Driver</span><span class="keyword">\</span><span class="default">Exception</span><span class="keyword">\</span><span class="default">ConnectionTimeoutException $e</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="comment">// PHP cannot find a MongoDB server using the MongoDB connection string specified<br>&#xA0; &#xA0; // do something here<br></span><span class="keyword">}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Well most of the tutorials didn&apos;t explained well, So i hope this might help someone <br>Note: this is a part of my laravel project&#xA0; <br><br>//getting data from a collection<br><span class="default">&lt;?php<br><br></span><span class="keyword">use </span><span class="default">MongoDB</span><span class="keyword">\</span><span class="default">Client </span><span class="keyword">as </span><span class="default">Mongo</span><span class="keyword">;<br><br></span><span class="default">$user </span><span class="keyword">= </span><span class="string">&quot;admin&quot;</span><span class="keyword">;<br></span><span class="default">$pwd </span><span class="keyword">= </span><span class="string">&apos;password&apos;</span><span class="keyword">;<br><br></span><span class="default">$mongo </span><span class="keyword">= new </span><span class="default">Mongo</span><span class="keyword">(</span><span class="string">&quot;mongodb://</span><span class="keyword">${</span><span class="default">user</span><span class="keyword">}</span><span class="string">:</span><span class="keyword">${</span><span class="default">pwd</span><span class="keyword">}</span><span class="string">@127.0.0.1:27017&quot;</span><span class="keyword">);<br></span><span class="default">$collection </span><span class="keyword">= </span><span class="default">$mongo</span><span class="keyword">-&gt;</span><span class="default">db_name</span><span class="keyword">-&gt;</span><span class="default">collection</span><span class="keyword">;<br></span><span class="default">$result </span><span class="keyword">= </span><span class="default">$collection</span><span class="keyword">-&gt;</span><span class="default">find</span><span class="keyword">()-&gt;</span><span class="default">toArray</span><span class="keyword">();<br><br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">);<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mongodb.tutorial.library.php)

**[⬆ to root](/)**