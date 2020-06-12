# PDOStatement::errorCode




<div class="phpcode"><span class="html">
Statement&apos;s errorCode() returns an empty string before execution, and &apos;00000&apos; (five zeros) after a sucessfull execution:<br><br><span class="default">&lt;?php<br>$stmt </span><span class="keyword">= </span><span class="default">$pdo</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(</span><span class="default">$query</span><span class="keyword">);<br></span><span class="default">assert</span><span class="keyword">(</span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">errorCode </span><span class="keyword">=== </span><span class="string">&apos;&apos;</span><span class="keyword">);<br><br></span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">();<br></span><span class="default">assert</span><span class="keyword">(</span><span class="default">$stmt</span><span class="keyword">-&gt;</span><span class="default">errorCode </span><span class="keyword">=== </span><span class="string">&apos;00000&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.errorcode.php)

**[To root](/README.md)**