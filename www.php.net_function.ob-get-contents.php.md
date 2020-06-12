# ob_get_contents




<div class="phpcode"><span class="html">
This is an example of how the stack works:<br><br><span class="default">&lt;?php<br></span><span class="comment">//Level 0<br></span><span class="default">ob_start</span><span class="keyword">();<br>echo </span><span class="string">&quot;Hello &quot;</span><span class="keyword">;<br><br></span><span class="comment">//Level 1<br></span><span class="default">ob_start</span><span class="keyword">();<br>echo </span><span class="string">&quot;Hello World&quot;</span><span class="keyword">;<br></span><span class="default">$out2 </span><span class="keyword">= </span><span class="default">ob_get_contents</span><span class="keyword">();<br></span><span class="default">ob_end_clean</span><span class="keyword">();<br><br></span><span class="comment">//Back to level 0<br></span><span class="keyword">echo </span><span class="string">&quot;Galaxy&quot;</span><span class="keyword">;<br></span><span class="default">$out1 </span><span class="keyword">= </span><span class="default">ob_get_contents</span><span class="keyword">();<br></span><span class="default">ob_end_clean</span><span class="keyword">();<br><br></span><span class="comment">//Just output<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$out1</span><span class="keyword">, </span><span class="default">$out2</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ob-get-contents.php)

**[To root](/)**