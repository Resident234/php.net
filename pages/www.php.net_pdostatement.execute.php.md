# PDOStatement::execute




<div class="phpcode"><span class="html">
Hopefully this saves time for folks: one should use $count = $stmt-&gt;rowCount() after $stmt-&gt;execute() in order to really determine if any an operation such as &apos; update &apos; or &apos; replace &apos; did succeed i.e. changed some data.<br><br>Jean-Lou Dupont.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that you must
<br>- EITHER pass all values to bind in an array to PDOStatement::execute()
<br>- OR bind every value before with PDOStatement::bindValue(), then call PDOStatement::execute() with *no* parameter (not even &quot;array()&quot;!).
<br>Passing an array (empty or not) to execute() will &quot;erase&quot; and replace any previous bindings (and can lead to, e.g. with MySQL, &quot;SQLSTATE[HY000]: General error: 2031&quot; (CR_PARAMS_NOT_BOUND) if you passed an empty array).
<br>
<br>Thus the following function is incorrect in case the prepared statement has been &quot;bound&quot; before:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">customExecute</span><span class="keyword">(</span><span class="default">PDOStatement </span><span class="keyword">&amp;</span><span class="default">$sth</span><span class="keyword">, </span><span class="default">$params </span><span class="keyword">= </span><span class="default">NULL</span><span class="keyword">) {
<br>&#xA0; &#xA0; return </span><span class="default">$sth</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">(</span><span class="default">$params</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>and should therefore be replaced by something like:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">customExecute</span><span class="keyword">(</span><span class="default">PDOStatement </span><span class="keyword">&amp;</span><span class="default">$sth</span><span class="keyword">, array </span><span class="default">$params </span><span class="keyword">= array()) {
<br>&#xA0; &#xA0; if (empty(</span><span class="default">$params</span><span class="keyword">))
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$sth</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">();
<br>&#xA0; &#xA0; return </span><span class="default">$sth</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">(</span><span class="default">$params</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Also note that PDOStatement::execute() doesn&apos;t require $input_parameters to be an array.
<br>
<br>(of course, do not use it as is ^^).</span>
</div>
  

#


<div class="phpcode"><span class="html">
An array of insert values (named parameters) don&apos;t need the prefixed colon als key-value to work.<br><br><span class="default">&lt;?php<br></span><span class="comment">/* Execute a prepared statement by passing an array of insert values */<br></span><span class="default">$calories </span><span class="keyword">= </span><span class="default">150</span><span class="keyword">;<br></span><span class="default">$colour </span><span class="keyword">= </span><span class="string">&apos;red&apos;</span><span class="keyword">;<br></span><span class="default">$sth </span><span class="keyword">= </span><span class="default">$dbh</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="string">&apos;SELECT name, colour, calories<br>&#xA0;&#xA0; FROM fruit<br>&#xA0;&#xA0; WHERE calories &lt; :calories AND colour = :colour&apos;</span><span class="keyword">);<br></span><span class="comment">// instead of:<br>//&#xA0; &#xA0;&#xA0; $sth-&gt;execute(array(&apos;:calories&apos; =&gt; $calories, &apos;:colour&apos; =&gt; $colour));<br>// this works fine, too:<br></span><span class="default">$sth</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">(array(</span><span class="string">&apos;calories&apos; </span><span class="keyword">=&gt; </span><span class="default">$calories</span><span class="keyword">, </span><span class="string">&apos;colour&apos; </span><span class="keyword">=&gt; </span><span class="default">$colour</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>This allows to use &quot;regular&quot; assembled hash-tables (arrays).<br>That realy does make sense!</span>
</div>
  

#


<div class="phpcode"><span class="html">
simplified $placeholder form 
<br>
<br><span class="default">&lt;?php
<br>
<br>$data </span><span class="keyword">= [</span><span class="string">&apos;a&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;foo&apos;</span><span class="keyword">,</span><span class="string">&apos;b&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;bar&apos;</span><span class="keyword">];
<br>
<br></span><span class="default">$keys </span><span class="keyword">= </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);
<br></span><span class="default">$fields </span><span class="keyword">= </span><span class="string">&apos;`&apos;</span><span class="keyword">.</span><span class="default">implode</span><span class="keyword">(</span><span class="string">&apos;`, `&apos;</span><span class="keyword">,</span><span class="default">$keys</span><span class="keyword">).</span><span class="string">&apos;`&apos;</span><span class="keyword">;
<br>
<br></span><span class="comment">#here is my way 
<br></span><span class="default">$placeholder </span><span class="keyword">= </span><span class="default">substr</span><span class="keyword">(</span><span class="default">str_repeat</span><span class="keyword">(</span><span class="string">&apos;?,&apos;</span><span class="keyword">,</span><span class="default">count</span><span class="keyword">(</span><span class="default">$keys</span><span class="keyword">)),</span><span class="default">0</span><span class="keyword">,-</span><span class="default">1</span><span class="keyword">);
<br>
<br></span><span class="default">$pdo</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="string">&quot;INSERT INTO `baz`(</span><span class="default">$fields</span><span class="string">) VALUES(</span><span class="default">$placeholder</span><span class="string">)&quot;</span><span class="keyword">)-&gt;</span><span class="default">execute</span><span class="keyword">(</span><span class="default">array_values</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">));</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
When using a prepared statement to execute multiple inserts (such as in a loop etc), under sqlite the performance is dramatically improved by wrapping the loop in a transaction.
<br>
<br>I have an application that routinely inserts 30-50,000 records at a time.&#xA0; Without the transaction it was taking over 150 seconds, and with it only 3.
<br>
<br>This may affect other implementations as well, and I am sure it is something that affects all databases to some extent, but I can only test with PDO sqlite.
<br>
<br>e.g.
<br>
<br><span class="default">&lt;?php
<br>$data </span><span class="keyword">= array(
<br>&#xA0; array(</span><span class="string">&apos;name&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;John&apos;</span><span class="keyword">, </span><span class="string">&apos;age&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;25&apos;</span><span class="keyword">),
<br>&#xA0; array(</span><span class="string">&apos;name&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;Wendy&apos;</span><span class="keyword">, </span><span class="string">&apos;age&apos; </span><span class="keyword">=&gt; </span><span class="string">&apos;32&apos;</span><span class="keyword">)
<br>);
<br>
<br>try {
<br>&#xA0; </span><span class="default">$pdo </span><span class="keyword">= new </span><span class="default">PDO</span><span class="keyword">(</span><span class="string">&apos;sqlite:myfile.sqlite&apos;</span><span class="keyword">);
<br>}
<br>
<br>catch(</span><span class="default">PDOException $e</span><span class="keyword">) {
<br>&#xA0; die(</span><span class="string">&apos;Unable to open database connection&apos;</span><span class="keyword">);
<br>}
<br>
<br></span><span class="default">$insertStatement </span><span class="keyword">= </span><span class="default">$pdo</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="string">&apos;insert into mytable (name, age) values (:name, :age)&apos;</span><span class="keyword">);
<br>
<br></span><span class="comment">// start transaction
<br></span><span class="default">$pdo</span><span class="keyword">-&gt;</span><span class="default">beginTransaction</span><span class="keyword">();
<br>
<br>foreach(</span><span class="default">$data </span><span class="keyword">as &amp;</span><span class="default">$row</span><span class="keyword">) {
<br>&#xA0; </span><span class="default">$insertStatement</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">(</span><span class="default">$row</span><span class="keyword">);
<br>}
<br>
<br></span><span class="comment">// end transaction
<br></span><span class="default">$pdo</span><span class="keyword">-&gt;</span><span class="default">commit</span><span class="keyword">();
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>[EDITED BY sobak: typofixes by Pere submitted on 12-Sep-2014 01:07]</span>
</div>
  

#


<div class="phpcode"><span class="html">
When passing an array of values to execute when your query contains question marks, note that the array must be keyed numerically from zero. If it is not, run array_values() on it to force the array to be re-keyed.<br><br><span class="default">&lt;?php<br>$anarray </span><span class="keyword">= array(</span><span class="default">42 </span><span class="keyword">=&gt; </span><span class="string">&quot;foo&quot;</span><span class="keyword">, </span><span class="default">101 </span><span class="keyword">=&gt; </span><span class="string">&quot;bar&quot;</span><span class="keyword">);<br></span><span class="default">$statement </span><span class="keyword">= </span><span class="default">$dbo</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="string">&quot;SELECT * FROM table WHERE col1 = ? AND col2 = ?&quot;</span><span class="keyword">);<br><br></span><span class="comment">//This will not work<br></span><span class="default">$statement</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">(</span><span class="default">$anarray</span><span class="keyword">);<br><br></span><span class="comment">//Do this to make it work<br></span><span class="default">$statement</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">(</span><span class="default">array_values</span><span class="keyword">(</span><span class="default">$anarray</span><span class="keyword">));<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.execute.php)

**[To root](/README.md)**