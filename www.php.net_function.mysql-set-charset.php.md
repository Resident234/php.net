# mysql_set_charset




<div class="phpcode"><span class="html">
I needed to access the database from within one particular webhosting service. Pages are UTF-8 encoded and data received by forms should be inserted into database without changing the encoding. The database is also in UTF-8.<br><br>Neither SET character set &apos;utf8&apos; or SET names &apos;utf8&apos; worked properly here, so this workaround made sure all variables are set to utf-8.<br><br><span class="default">&lt;?php<br><br></span><span class="comment">// ... (creating a connection to mysql) ...<br><br></span><span class="default">mysql_query</span><span class="keyword">(</span><span class="string">&quot;SET character_set_results = &apos;utf8&apos;, character_set_client = &apos;utf8&apos;, character_set_connection = &apos;utf8&apos;, character_set_database = &apos;utf8&apos;, character_set_server = &apos;utf8&apos;&quot;</span><span class="keyword">, </span><span class="default">$conn</span><span class="keyword">);<br><br></span><span class="default">$re </span><span class="keyword">= </span><span class="default">mysql_query</span><span class="keyword">(</span><span class="string">&apos;SHOW VARIABLES LIKE &quot;%character_set%&quot;;&apos;</span><span class="keyword">)or die(</span><span class="default">mysql_error</span><span class="keyword">());<br>while (</span><span class="default">$r </span><span class="keyword">= </span><span class="default">mysql_fetch_assoc</span><span class="keyword">(</span><span class="default">$re</span><span class="keyword">)) {</span><span class="default">var_dump </span><span class="keyword">(</span><span class="default">$r</span><span class="keyword">); echo </span><span class="string">&quot;&lt;br /&gt;&quot;</span><span class="keyword">;} exit;<br><br></span><span class="default">?&gt;<br></span><br>All important variables are now utf-8 and we can safely use INSERTs or SELECTs with mysql_escape_string($var) without any encoding functions.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here&apos;s an example of how to use this feature :<br><br>I&apos;m using&#xA0; PHP 5.2.5 from <a href="http://oss.oracle.com/projects/php/" rel="nofollow" target="_blank">http://oss.oracle.com/projects/php/</a><br><br>I wanted to store arabic characters as UTF8 in the database and as suggested in many of the google results, I tried using<br><br>mysql_query(&quot;SET NAMES &apos;utf8&apos;&quot;);<br><br>and <br><br>mysql_query(&quot;SET CHARACTER SET utf8 &quot;);<br><br>but it did not work.<br><br>Using the following, it worked flawlessly<br><br>$link = mysql_connect(&apos;localhost&apos;, &apos;user&apos;, &apos;password&apos;);<br>mysql_set_charset(&apos;utf8&apos;,$link);<br><br>Once this is set we need not manually encode the text into utf using utf8_encode() or other functions. The arabic ( or any UTF8 supported ) text can be passed directly to the database and it is automatically converted by PHP.<br>For eg.<br><span class="default">&lt;?php<br>$link </span><span class="keyword">= </span><span class="default">mysql_connect</span><span class="keyword">(</span><span class="string">&apos;localhost&apos;</span><span class="keyword">, </span><span class="string">&apos;user&apos;</span><span class="keyword">, </span><span class="string">&apos;password&apos;</span><span class="keyword">);<br></span><span class="default">mysql_set_charset</span><span class="keyword">(</span><span class="string">&apos;utf8&apos;</span><span class="keyword">,</span><span class="default">$link</span><span class="keyword">);<br></span><span class="default">$db_selected </span><span class="keyword">= </span><span class="default">mysql_select_db</span><span class="keyword">(</span><span class="string">&apos;emp_feedback&apos;</span><span class="keyword">, </span><span class="default">$link</span><span class="keyword">);<br>if (!</span><span class="default">$db_selected</span><span class="keyword">) { die (</span><span class="string">&apos;Database access error : &apos; </span><span class="keyword">. </span><span class="default">mysql_error</span><span class="keyword">());}<br></span><span class="default">$query </span><span class="keyword">= </span><span class="string">&quot;INSERT INTO feedback ( EmpName, Message ) VALUES (&apos;</span><span class="default">$_empName</span><span class="string">&apos;,&apos;</span><span class="default">$_message</span><span class="string">&apos;)&quot;</span><span class="keyword">;<br></span><span class="default">mysql_query</span><span class="keyword">(</span><span class="default">$query</span><span class="keyword">) or die(</span><span class="string">&apos;Error, Feedback insert into database failed&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span>Note that here $_empName is stored in English while $_message is in Arabic.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.mysql-set-charset.php)

**[To root](/README.md)**