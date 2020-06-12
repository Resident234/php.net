# settype




<div class="phpcode"><span class="html">
Note that you can&apos;t use this to convert a string &apos;true&apos; or &apos;false&apos; to a boolean variable true or false as a string &apos;false&apos; is a boolean true. The empty string would be false instead...<br><br><span class="default">&lt;?php<br>$var </span><span class="keyword">= </span><span class="string">&quot;true&quot;</span><span class="keyword">;<br></span><span class="default">settype</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">, </span><span class="string">&apos;bool&apos;</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">); </span><span class="comment">// true<br><br></span><span class="default">$var </span><span class="keyword">= </span><span class="string">&quot;false&quot;</span><span class="keyword">;<br></span><span class="default">settype</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">, </span><span class="string">&apos;bool&apos;</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">); </span><span class="comment">// true as well!<br><br></span><span class="default">$var </span><span class="keyword">= </span><span class="string">&quot;&quot;</span><span class="keyword">;<br></span><span class="default">settype</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">, </span><span class="string">&apos;bool&apos;</span><span class="keyword">);<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$var</span><span class="keyword">); </span><span class="comment">// false<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Just a quick note, as this caught me out very briefly:<br><br>settype() returns bool, not the typecasted variable - so:<br><br>$blah = settype($blah, &quot;int&quot;); // is wrong, changes $blah to 0 or 1<br>settype($blah, &quot;int&quot;); // is correct<br><br>Hope this helps someone else who makes a mistake.. ;)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Using settype is not the best way to convert a string into an integer, since it will strip the string wherever the first non-numeric character begins.&#xA0; The function intval($string) does the same thing.<br><br>If you&apos;re looking for a security check, or to strip non-numeric characters (such as cleaning up phone numbers or ZIP codes),&#xA0; try this instead:<br><br>&lt;?<br>&#xA0; &#xA0;&#xA0; $number=ereg_replace(&quot;[^0-9]&quot;,&quot;&quot;,$number);<br>?&gt;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.settype.php)

**[⬆ to root](/)**