# is_string




<div class="phpcode"><span class="html">
Using is_string() on an object will always return false (even with __toString()).<br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">B </span><span class="keyword">{<br>&#xA0; public function </span><span class="default">__toString</span><span class="keyword">() {<br>&#xA0; &#xA0; return </span><span class="string">&quot;Instances of B() can be treated as a strings!\n&quot;</span><span class="keyword">;<br>&#xA0; }<br>}&#xA0; <br><br></span><span class="default">$b </span><span class="keyword">= new </span><span class="default">B</span><span class="keyword">();<br>print(</span><span class="default">$b</span><span class="keyword">); </span><span class="comment">//Instances of B() can be treated as a strings!<br></span><span class="keyword">print(</span><span class="default">is_string</span><span class="keyword">(</span><span class="default">$b</span><span class="keyword">) ? </span><span class="string">&apos;true&apos; </span><span class="keyword">: </span><span class="string">&apos;false&apos;</span><span class="keyword">); </span><span class="comment">//false<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.is-string.php)

**[To root](/README.md)**