# PDO::beginTransaction




<div class="phpcode"><span class="html">
The nested transaction example here is great, but it&apos;s missing a key piece of the puzzle.&#xA0; Commits will commit everything, I only wanted commits to actually commit when the outermost commit has been completed.&#xA0; This can be done in InnoDB with savepoints.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">Database </span><span class="keyword">extends </span><span class="default">PDO<br></span><span class="keyword">{<br><br>&#xA0; &#xA0; protected </span><span class="default">$transactionCount </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;<br><br>&#xA0; &#xA0; public function </span><span class="default">beginTransaction</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">transactionCounter</span><span class="keyword">++) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">parent</span><span class="keyword">::</span><span class="default">beginTransaction</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">exec</span><span class="keyword">(</span><span class="string">&apos;SAVEPOINT trans&apos;</span><span class="keyword">.</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">transactionCounter</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">transactionCounter </span><span class="keyword">&gt;= </span><span class="default">0</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; public function </span><span class="default">commit</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!--</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">transactionCounter</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">parent</span><span class="keyword">::</span><span class="default">commit</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">transactionCounter </span><span class="keyword">&gt;= </span><span class="default">0</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; public function </span><span class="default">rollback</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (--</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">transactionCounter</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">exec</span><span class="keyword">(</span><span class="string">&apos;ROLLBACK TO trans&apos;</span><span class="keyword">.</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">transactionCounter </span><span class="keyword">+ </span><span class="default">1</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">parent</span><span class="keyword">::</span><span class="default">rollback</span><span class="keyword">();<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>}</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can generate problems with nested beginTransaction and commit calls.
<br>example:
<br>
<br>beginTransaction()
<br>do imprortant stuff
<br>call method
<br>&#xA0; &#xA0; beginTransaction()
<br>&#xA0; &#xA0; basic stuff 1
<br>&#xA0; &#xA0; basic stuff 2
<br>&#xA0; &#xA0; commit()
<br>do most important stuff
<br>commit()
<br>
<br>Won&apos;t work and is dangerous since you could close your transaction too early with the nested commit().
<br>
<br>There is no need to mess you code and pass like a bool which indicate if transaction is already running. You could just overload the beginTransaction() and commit() in your PDO wrapper like this:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">class </span><span class="default">Database </span><span class="keyword">extends \\</span><span class="default">PDO
<br></span><span class="keyword">{
<br>&#xA0; &#xA0; protected </span><span class="default">$transactionCounter </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; function </span><span class="default">beginTransaction</span><span class="keyword">()
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if(!</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">transactionCounter</span><span class="keyword">++)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">parent</span><span class="keyword">::</span><span class="default">beginTransaction</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0;&#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">transactionCounter </span><span class="keyword">&gt;= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; function </span><span class="default">commit</span><span class="keyword">()
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0;&#xA0; if(!--</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">transactionCounter</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return </span><span class="default">parent</span><span class="keyword">::</span><span class="default">commit</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0;&#xA0; return </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">transactionCounter </span><span class="keyword">&gt;= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; function </span><span class="default">rollback</span><span class="keyword">()
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">transactionCounter </span><span class="keyword">&gt;= </span><span class="default">0</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">transactionCounter </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">parent</span><span class="keyword">::</span><span class="default">rollback</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">transactionCounter </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br></span><span class="comment">//...
<br></span><span class="keyword">}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
In response to &quot;Anonymous / 20-Dec-2007 03:04&quot;<br><br>You could also extend the PDO class and hold a private flag to check if a transaction is already started.<br><br>class MyPDO extends PDO {<br>&#xA0;&#xA0; protected $hasActiveTransaction = false;<br><br>&#xA0;&#xA0; function beginTransaction () {<br>&#xA0; &#xA0; &#xA0; if ( $this-&gt;hasActiveTransaction ) {<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return false;<br>&#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $this-&gt;hasActiveTransaction = parent::beginTransaction ();<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return $this-&gt;hasActiveTransaction;<br>&#xA0; &#xA0; &#xA0; }<br>&#xA0;&#xA0; }<br><br>&#xA0;&#xA0; function commit () {<br>&#xA0; &#xA0; &#xA0; parent::commit ();<br>&#xA0; &#xA0; &#xA0; $this-&gt;hasActiveTransaction = false;<br>&#xA0;&#xA0; }<br><br>&#xA0;&#xA0; function rollback () {<br>&#xA0; &#xA0; &#xA0; parent::rollback ();<br>&#xA0; &#xA0; &#xA0; $this-&gt;hasActiveTransaction = false;<br>&#xA0;&#xA0; }<br><br>}</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdo.begintransaction.php)

**[To root](/README.md)**