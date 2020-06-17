# PDO::errorCode




<div class="phpcode"><span class="html">
Hi,
<br>
<br>List containing all SQL-92 SQLSTATE Return Codes:
<br><a href="http://www.unix.org.ua/orelly/java-ent/jenut/ch08_06.htm" rel="nofollow" target="_blank">http://www.unix.org.ua/orelly/java-ent/jenut/ch08_06.htm</a>
<br>
<br>Use the following Code to let PDO throw Exceptions (PDOException) on Error.
<br>
<br><span class="default">&lt;?PHP 
<br>$pdo </span><span class="keyword">= new </span><span class="default">PDO </span><span class="keyword">(</span><span class="default">whatever</span><span class="keyword">);
<br></span><span class="default">$pdo</span><span class="keyword">-&gt;</span><span class="default">setAttribute</span><span class="keyword">(</span><span class="default">PDO</span><span class="keyword">::</span><span class="default">ATTR_ERRMODE</span><span class="keyword">, </span><span class="default">PDO</span><span class="keyword">::</span><span class="default">ERRMODE_EXCEPTION</span><span class="keyword">);
<br>try {
<br>&#xA0; &#xA0; </span><span class="default">$pdo</span><span class="keyword">-&gt;</span><span class="default">exec </span><span class="keyword">(</span><span class="string">&quot;QUERY WITH SYNTAX ERROR&quot;</span><span class="keyword">);
<br>} catch (</span><span class="default">PDOException $e</span><span class="keyword">) {
<br>&#xA0; &#xA0; if (</span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">getCode</span><span class="keyword">() == </span><span class="string">&apos;2A000&apos;</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;Syntax Error: &quot;</span><span class="keyword">.</span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">getMessage</span><span class="keyword">();
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Bye,
<br>&#xA0; Matthias</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdo.errorcode.php)

**[To root](/README.md)**