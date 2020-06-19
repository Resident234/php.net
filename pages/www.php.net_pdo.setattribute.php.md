# PDO::setAttribute




<div class="phpcode"><span class="html">
Because no examples are provided, and to alleviate any confusion as a result, the setAttribute() method is invoked like so:<br><br>setAttribute(ATTRIBUTE, OPTION);<br><br>So, if I wanted to ensure that the column names returned from a query were returned in the case the database driver returned them (rather than having them returned in all upper case [as is the default on some of the PDO extensions]), I would do the following:<br><br><span class="default">&lt;?php<br></span><span class="comment">// Create a new database connection.<br></span><span class="default">$dbConnection </span><span class="keyword">= new </span><span class="default">PDO</span><span class="keyword">(</span><span class="default">$dsn</span><span class="keyword">, </span><span class="default">$user</span><span class="keyword">, </span><span class="default">$pass</span><span class="keyword">);<br><br></span><span class="comment">// Set the case in which to return column_names.<br></span><span class="default">$dbConnection</span><span class="keyword">-&gt;</span><span class="default">setAttribute</span><span class="keyword">(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">ATTR_CASE</span><span class="keyword">, </span><span class="default">PDO</span><span class="keyword">::</span><span class="default">CASE_NATURAL</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Hope this helps some of you who learn by example (as is the case with me).<br><br>.Colin</span>
</div>
  

#


<div class="phpcode"><span class="html">
This is an update to a note I wrote earlier concerning how to set multiple attributes when you create you PDO connection string.<br><br>You can put all the attributes you want in an associative array and pass that array as the fourth parameter in your connection string. So it goes like this: <br><span class="default">&lt;?php<br>$options </span><span class="keyword">= [<br>&#xA0; &#xA0; </span><span class="default">PDO</span><span class="keyword">::</span><span class="default">ATTR_ERRMODE </span><span class="keyword">=&gt; </span><span class="default">PDO</span><span class="keyword">::</span><span class="default">ERRMODE_EXCEPTION</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="default">PDO</span><span class="keyword">::</span><span class="default">ATTR_CASE </span><span class="keyword">=&gt; </span><span class="default">PDO</span><span class="keyword">::</span><span class="default">CASE_NATURAL</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="default">PDO</span><span class="keyword">::</span><span class="default">ATTR_ORACLE_NULLS </span><span class="keyword">=&gt; </span><span class="default">PDO</span><span class="keyword">::</span><span class="default">NULL_EMPTY_STRING<br></span><span class="keyword">];<br><br></span><span class="comment">// Now you create your connection string<br></span><span class="keyword">try {<br>&#xA0; &#xA0; </span><span class="comment">// Then pass the options as the last parameter in the connection string<br>&#xA0; &#xA0; </span><span class="default">$connection </span><span class="keyword">= new </span><span class="default">PDO</span><span class="keyword">(</span><span class="string">&quot;mysql:host=</span><span class="default">$host</span><span class="string">; dbname=</span><span class="default">$dbname</span><span class="string">&quot;</span><span class="keyword">, </span><span class="default">$user</span><span class="keyword">, </span><span class="default">$password</span><span class="keyword">, </span><span class="default">$options</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="comment">// That&apos;s how you can set multiple attributes<br></span><span class="keyword">} catch(</span><span class="default">PDOException $e</span><span class="keyword">) {<br>&#xA0; &#xA0; die(</span><span class="string">&quot;Database connection failed: &quot; </span><span class="keyword">. </span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">getMessage</span><span class="keyword">());<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Well, I have not seen it mentioned anywhere and thought its worth mentioning. It might help someone. If you are wondering whether you can set multiple attributes then the answer is yes.<br><br>You can do it like this:<br>try {<br>&#xA0; &#xA0; $connection = new PDO(&quot;mysql:host=$host; dbname=$dbname&quot;, $user, $password);<br>&#xA0; &#xA0; // You can begin setting all the attributes you want.<br>&#xA0; &#xA0; $connection-&gt;setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);<br>&#xA0; &#xA0; $connection-&gt;setAttribute(PDO::ATTR_CASE, PDO::CASE_NATURAL);<br>&#xA0; &#xA0; $connection-&gt;setAttribute(PDO::ATTR_ORACLE_NULLS, PDO::NULL_EMPTY_STRING);<br><br>&#xA0; &#xA0; // That&apos;s how you can set multiple attributes<br>}<br>catch(PDOException $e)<br>{<br>&#xA0; &#xA0; die(&quot;Database connection failed: &quot; . $e-&gt;getMessage());<br>}<br><br>I hope this helps somebody. :)</span>
</div>
  

#


<div class="phpcode"><span class="html">
It is worth noting that not all attributes may be settable via setAttribute().&#xA0; For example, PDO::MYSQL_ATTR_MAX_BUFFER_SIZE is only settable in PDO::__construct().&#xA0; You must pass PDO::MYSQL_ATTR_MAX_BUFFER_SIZE as part of the optional 4th parameter to the constructor.&#xA0; This is detailed in <a href="http://bugs.php.net/bug.php?id=38015" rel="nofollow" target="_blank">http://bugs.php.net/bug.php?id=38015</a></span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdo.setattribute.php)

**[To root](/README.md)**