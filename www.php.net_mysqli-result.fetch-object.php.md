# mysqli_result::fetch_object




<div class="phpcode"><span class="html">
Please mind the difference between objects and arrays in PHP&gt;=5: arrays are by value while objects are by reference.<br><br>&lt;?<br>$o = mysqli_fetch_object($res);<br>$o1 = $o;<br>$o1-&gt;value = 10;<br><br>$a = mysqli_fetch_array($res);<br>$a1 = $a;<br>$a1[&apos;value&apos;] = 10;<br><br>echo $o-&gt;value; // 10<br>echo $a[&apos;value&apos;]; // (original value from db)<br>?&gt;<br><br>Should same behaviour be intended, the object needs to be cloned:<br><br>&lt;?<br>$o1 = clone $o;<br>?&gt;<br><br>More about object cloning:<br><a href="http://php.net/manual/en/language.oop5.cloning.php" rel="nofollow" target="_blank">http://php.net/manual/en/language.oop5.cloning.php</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
Since 5.6.21 and PHP 7.0.6<br><br>mysqli_fetch_object() sets the properties of the object AFTER calling the object constructor. Not BEFORE as was in previous versions.<br><br>So behaviour has changed. Seems a bug but not sure if was done intentionally.<br><br><a href="https://bugs.php.net/bug.php?id=72151" rel="nofollow" target="_blank">https://bugs.php.net/bug.php?id=72151</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
As indicated in the user comments of the mysql_fetch_object, it is important to realize that class fields get values assigned to them BEFORE the constructor is called.<br>For example;<br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">Employee<br></span><span class="keyword">{<br>&#xA0; private </span><span class="default">$id</span><span class="keyword">;<br><br>&#xA0; public function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$id </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">)<br>&#xA0; {<br>&#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">id </span><span class="keyword">= </span><span class="default">$id</span><span class="keyword">;<br>&#xA0; }<br>}<br><br></span><span class="comment">// some code for creating a database connection... i.e. mysqli object<br></span><span class="keyword">....<br></span><span class="default">$result </span><span class="keyword">= </span><span class="default">$con</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;select id, name from employees&quot;</span><span class="keyword">);<br></span><span class="default">$anEmployee </span><span class="keyword">= </span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">fetch_object</span><span class="keyword">(</span><span class="string">&quot;Employee&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span>will result in the ID being 0 because it is overridden by the constructor. Therefore, it is useful to check if the class field is already set.<br>I.e.<br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">Employee<br></span><span class="keyword">{<br>&#xA0; private </span><span class="default">$id</span><span class="keyword">;<br><br>&#xA0; public function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$id </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">)<br>&#xA0; {<br>&#xA0; &#xA0; if (!</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">id</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">id </span><span class="keyword">= </span><span class="default">$id <br>&#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; }<br>}<br></span><span class="default">?&gt;<br></span>Also note that the fields which will be assigned by fetch_object are case sensitive. If your table has the field &quot;ID&quot;, it will result in the class field $ID being set. A simple work-around is to use aliases. I.e. &quot;SELECT *, ID as id FROM myTable&quot;<br>I hope this helps some people.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-result.fetch-object.php)

**[⬆ to root](/)**