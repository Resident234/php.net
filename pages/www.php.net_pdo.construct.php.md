# PDO::__construct




<div class="phpcode"><span class="html">
To get UTF-8 charset you can specify that in the DSN.<br><br>$link = new PDO(&quot;mysql:host=localhost;dbname=DB;charset=UTF8&quot;);</span>
</div>
  

#


<div class="phpcode"><span class="html">
I&apos;d like to point out that in PHP 7.0 in the dsn parameter you can&apos;t use &apos;host=localhost&apos; to solve this you can use &apos;host=127.0.0.1&apos; instead.</span>
</div>
  

#


<div class="phpcode"><span class="html">
To specify a database connection port use the following DSN string<br><br><span class="default">&lt;?php<br>$dsn </span><span class="keyword">= </span><span class="string">&apos;mysql:dbname=testdb;host=127.0.0.1;port=3333&apos;</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
To connect throught unix socket you need to use <br><span class="default">&lt;?php<br>$dsn </span><span class="keyword">= </span><span class="string">&apos;mysql:dbname=testdb;unix_socket=/path/to/socket&apos;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>You musn&apos;t specify host when using socket.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you use the UTF-8 encoding, you have to use the fourth parameter :
<br>
<br><span class="default">&lt;?php
<br>$db </span><span class="keyword">= new </span><span class="default">PDO</span><span class="keyword">(</span><span class="string">&apos;mysql:host=myhost;dbname=mydb&apos;</span><span class="keyword">, </span><span class="string">&apos;login&apos;</span><span class="keyword">, </span><span class="string">&apos;password&apos;</span><span class="keyword">, array(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">MYSQL_ATTR_INIT_COMMAND </span><span class="keyword">=&gt; </span><span class="string">&apos;SET NAMES \&apos;UTF8\&apos;&apos;</span><span class="keyword">));
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Sqlite:<br><br><span class="default">&lt;?php<br></span><span class="keyword">try{&#xA0; &#xA0;&#xA0; <br>&#xA0; &#xA0; </span><span class="default">$pdo </span><span class="keyword">= new </span><span class="default">PDO</span><span class="keyword">(</span><span class="string">&apos;sqlite:example.db&apos;</span><span class="keyword">);<br>}catch (</span><span class="default">PDOException $e</span><span class="keyword">){<br>&#xA0; &#xA0;&#xA0; die (</span><span class="string">&apos;DB Error&apos;</span><span class="keyword">);<br>}<br></span><span class="default">?&gt;<br></span><br>If &apos;example.db&apos; does not exist, no exception is thrown but the file &apos;example.db&apos; is created.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdo.construct.php)

**[To root](/README.md)**