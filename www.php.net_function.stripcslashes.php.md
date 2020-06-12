# stripcslashes




<div class="phpcode"><span class="html">
stripcslashes does not simply skip the C-style escape sequences \a, \b, \f, \n, \r, \t and \v, but converts them to their actual meaning. <br><br>So<br><span class="default">&lt;?php<br>stripcslashes</span><span class="keyword">(</span><span class="string">&apos;\n&apos;</span><span class="keyword">) == </span><span class="string">&quot;\n&quot;</span><span class="keyword">; </span><span class="comment">//true;<br><br></span><span class="default">$str </span><span class="keyword">= </span><span class="string">&quot;we are escaping \r\n&quot;</span><span class="keyword">; </span><span class="comment">//we are escaping<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.stripcslashes.php)

**[⬆ to root](/)**