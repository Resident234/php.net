# mysqli::autocommitmysqli_autocommit




<div class="phpcode"><span class="html">
Just to be clear, autocommit not only turns on/off transactions, but will also &apos;commit&apos; any waiting queries.<br><span class="default">&lt;?php<br>mysqli_autocommit</span><span class="keyword">(</span><span class="default">$link</span><span class="keyword">, </span><span class="default">FALSE</span><span class="keyword">); </span><span class="comment">// turn OFF auto<br></span><span class="keyword">-</span><span class="default">some query 1</span><span class="keyword">;<br>-</span><span class="default">some query 2</span><span class="keyword">;<br></span><span class="default">mysqli_commit</span><span class="keyword">(</span><span class="default">$link</span><span class="keyword">); </span><span class="comment">// process ALL queries so far<br></span><span class="keyword">-</span><span class="default">some query 3</span><span class="keyword">;<br>-</span><span class="default">some query 4</span><span class="keyword">;<br></span><span class="default">mysqli_autocommit</span><span class="keyword">(</span><span class="default">$link</span><span class="keyword">, </span><span class="default">TRUE</span><span class="keyword">); </span><span class="comment">// turn ON auto<br></span><span class="default">?&gt;<br></span>All 4 will be processed.</span>
</div>
  

#


<div class="phpcode"><span class="html">
It&apos;s worth noting that you can perform transactions without disabling autocommit just using standard sql. &quot;START TRANSACTION;&quot; will start a transaction. &quot;COMMIT;&quot; will commit the results and &quot;ROLLBACK;&quot; will revert to the pre-transaction state.<br><br>CREATE TABLE and CREATE DATABASE (and probably others) are always commited immediately and your transaction appears to terminate. Thus any commands before and after will be commited, even if a subsequent rollback is attempted.<br><br>If you are in the middle of a transaction and you call mysqli_close() it appears that you get the funcitonality of an implicit rollback.<br><br>I can&apos;t reproduce the &quot;code bug causes lock&quot; problem outlined below (I always get a successful rollback and the script will run umtine times successfully). Therefore, I would suggest that the problem is fixed in php-5.2.2.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.autocommit.php)

**[To root](/README.md)**