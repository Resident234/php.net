# PDO_SQLITE DSN




<div class="phpcode"><span class="html">
In memory sqlite has some limitations. The memory space could be the request, the session, but no way seems documented to share a base in memory among users.
<br>
<br>For a request, open your base with the code
<br>$pdo = new PDO( &apos;sqlite::memory:&apos;);
<br>and your base will disapear on next request.
<br>
<br>For session persistency
<br><span class="default">&lt;?php
<br>$pdo </span><span class="keyword">= new </span><span class="default">PDO</span><span class="keyword">(
<br>&#xA0; &#xA0; </span><span class="string">&apos;sqlite::memory:&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="default">null</span><span class="keyword">,
<br>&#xA0; &#xA0; </span><span class="default">null</span><span class="keyword">,
<br>&#xA0; &#xA0; array(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">ATTR_PERSISTENT </span><span class="keyword">=&gt; </span><span class="default">true</span><span class="keyword">)
<br>);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/ref.pdo-sqlite.connection.php)

**[To root](/README.md)**