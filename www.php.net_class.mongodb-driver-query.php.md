# The MongoDB\Driver\Query class




<div class="phpcode"><span class="html">
Find By _id <br><br>$mongo = new \MongoDB\Driver\Manager(&apos;mongodb://root:root@192.168.0.127/db&apos;);<br><br>$id&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = new \MongoDB\BSON\ObjectId(&quot;588c78ce02ac660426003d87&quot;);<br>$filter&#xA0; &#xA0; &#xA0; = [&apos;_id&apos; =&gt; $id];<br>$options = [];<br><br>$query = new \MongoDB\Driver\Query($filter, $options);<br>$rows&#xA0;&#xA0; = $mongo-&gt;executeQuery(&apos;db.collectionName&apos;, $query); <br><br>foreach ($rows as $document) {<br>&#xA0; pr($document);<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is an example of Query to retrieve the records from MangoDB collection using a filter. It will return in this case just one record that satisfy the filter id = 2.<br><br>Considering the following MangoDB collection:<br><br><span class="default">&lt;?php<br></span><span class="comment">/* my_collection */<br><br>/* 1 */<br></span><span class="keyword">{<br>&#xA0; &#xA0; </span><span class="string">&quot;_id&quot; </span><span class="keyword">: </span><span class="default">ObjectId</span><span class="keyword">(</span><span class="string">&quot;5707f007639a94cbc600f282&quot;</span><span class="keyword">),<br>&#xA0; &#xA0; </span><span class="string">&quot;id&quot; </span><span class="keyword">: </span><span class="default">1</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&quot;name&quot; </span><span class="keyword">: </span><span class="string">&quot;Name 1&quot;<br></span><span class="keyword">}<br><br></span><span class="comment">/* 2 */<br></span><span class="keyword">{<br>&#xA0; &#xA0; </span><span class="string">&quot;_id&quot; </span><span class="keyword">: </span><span class="default">ObjectId</span><span class="keyword">(</span><span class="string">&quot;5707f0a8639a94f4cd2c84b1&quot;</span><span class="keyword">),<br>&#xA0; &#xA0; </span><span class="string">&quot;id&quot; </span><span class="keyword">: </span><span class="default">2</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&quot;name&quot; </span><span class="keyword">: </span><span class="string">&quot;Name 2&quot;<br></span><span class="keyword">}<br></span><span class="default">?&gt;<br></span><br>I&apos;m using the following code:<br><span class="default">&lt;?php<br>$filter </span><span class="keyword">= [</span><span class="string">&apos;id&apos; </span><span class="keyword">=&gt; </span><span class="default">2</span><span class="keyword">];<br></span><span class="default">$options </span><span class="keyword">= [<br>&#xA0;&#xA0; </span><span class="string">&apos;projection&apos; </span><span class="keyword">=&gt; [</span><span class="string">&apos;_id&apos; </span><span class="keyword">=&gt; </span><span class="default">0</span><span class="keyword">],<br>];<br></span><span class="default">$query </span><span class="keyword">= new </span><span class="default">MongoDB</span><span class="keyword">\</span><span class="default">Driver</span><span class="keyword">\</span><span class="default">Query</span><span class="keyword">(</span><span class="default">$filter</span><span class="keyword">, </span><span class="default">$options</span><span class="keyword">);<br></span><span class="default">$rows </span><span class="keyword">= </span><span class="default">$mongo</span><span class="keyword">-&gt;</span><span class="default">executeQuery</span><span class="keyword">(</span><span class="string">&apos;db_name.my_collection&apos;</span><span class="keyword">, </span><span class="default">$query</span><span class="keyword">); </span><span class="comment">// $mongo contains the connection object to MongoDB<br></span><span class="keyword">foreach(</span><span class="default">$rows </span><span class="keyword">as </span><span class="default">$r</span><span class="keyword">){<br>&#xA0;&#xA0; </span><span class="default">print_</span><span class="keyword">(</span><span class="default">$r</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.mongodb-driver-query.php)

**[⬆ to root](/)**