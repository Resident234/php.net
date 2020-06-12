# PDOStatement::setFetchMode




<div class="phpcode"><span class="html">
if you want to fetch your result into a class (by using PDO::FETCH_CLASS) and want the constructor to be executed *before* PDO assings the object properties, you need to use the PDO::FETCH_PROPS_LATE constant:<br><br><span class="default">&lt;?php<br>$stmt </span><span class="keyword">= </span><span class="default">$pdo</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="string">&quot;your query&quot;</span><span class="keyword">);<br><br></span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">setFetchMode</span><span class="keyword">(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_CLASS</span><span class="keyword">|</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">FETCH_PROPS_LATE</span><span class="keyword">, </span><span class="string">&quot;className&quot;</span><span class="keyword">, </span><span class="default">$constructorArguments</span><span class="keyword">);<br><br></span><span class="comment"># pass parameters, if required by the query<br></span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">(</span><span class="default">$parameters</span><span class="keyword">);<br><br>foreach (</span><span class="default">$stmt </span><span class="keyword">as </span><span class="default">$row</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="comment">// do something with (each of) your object<br></span><span class="keyword">}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.setfetchmode.php)

**[To root](/README.md)**