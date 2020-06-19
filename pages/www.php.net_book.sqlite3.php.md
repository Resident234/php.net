# SQLite3




<div class="phpcode"><span class="html">
As of PHP 5.4 support for Sqlite2 has been removed. I have a large web app that was built with sqlite2 as the database backend and thus it exploded when I updated PHP. If you&apos;re in a similar situation I&apos;ve written a few wrapper functions that will allow your app to work whilst you convert the code to sqlite3. 
<br>
<br>Firstly convert your DB to an sqlite3 db. 
<br>
<br>sqlite OLD.DB .dump | sqlite3 NEW.DB
<br>
<br>Then add the following functions to your app:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">sqlite_open</span><span class="keyword">(</span><span class="default">$location</span><span class="keyword">,</span><span class="default">$mode</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; </span><span class="default">$handle </span><span class="keyword">= new </span><span class="default">SQLite3</span><span class="keyword">(</span><span class="default">$location</span><span class="keyword">);
<br>&#xA0; &#xA0; return </span><span class="default">$handle</span><span class="keyword">;
<br>}
<br>function </span><span class="default">sqlite_query</span><span class="keyword">(</span><span class="default">$dbhandle</span><span class="keyword">,</span><span class="default">$query</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; </span><span class="default">$array</span><span class="keyword">[</span><span class="string">&apos;dbhandle&apos;</span><span class="keyword">] = </span><span class="default">$dbhandle</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$array</span><span class="keyword">[</span><span class="string">&apos;query&apos;</span><span class="keyword">] = </span><span class="default">$query</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= </span><span class="default">$dbhandle</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="default">$query</span><span class="keyword">);
<br>&#xA0; &#xA0; return </span><span class="default">$result</span><span class="keyword">;
<br>}
<br>function </span><span class="default">sqlite_fetch_array</span><span class="keyword">(&amp;</span><span class="default">$result</span><span class="keyword">,</span><span class="default">$type</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; </span><span class="comment">#Get Columns
<br>&#xA0; &#xA0; </span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; while (</span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">columnName</span><span class="keyword">(</span><span class="default">$i</span><span class="keyword">))
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$columns</span><span class="keyword">[ ] = </span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">columnName</span><span class="keyword">(</span><span class="default">$i</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$i</span><span class="keyword">++;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="default">$resx </span><span class="keyword">= </span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">fetchArray</span><span class="keyword">(</span><span class="default">SQLITE3_ASSOC</span><span class="keyword">);
<br>&#xA0; &#xA0; return </span><span class="default">$resx</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>They&apos;re not perfect by any stretch but they seem to be working ok as a temporary measure while I convert the site. 
<br>Hope that helps someone</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/book.sqlite3.php)

**[To root](/README.md)**