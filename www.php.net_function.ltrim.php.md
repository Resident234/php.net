# ltrim




<div class="phpcode"><span class="html">
When using a $character_mask the trimming stops at the first character that is not on that mask.<br><br>So in the $string = &quot;Hello world&quot; example with $character_mask = &quot;Hdle&quot;, ltrim($hello, $character_mask) goes like this:<br>1. Check H from &quot;Hello world&quot; =&gt; it is in the $character_mask, so remove it<br>2. Check e from &quot;ello world&quot; =&gt; it is in the $character_mask, so remove it<br>3. Check l from &quot;llo world&quot; =&gt; it is in the $character_mask, so remove it<br>4. Check l from &quot;lo world&quot; =&gt; it is in the $character_mask, so remove it<br>5. Check o from &quot;o world&quot; =&gt; it is NOT in the $character_mask, exit the function<br><br>Remaining string is &quot;o world&quot;.<br><br>I hope it helps someone as I had a confusing moment with this function.</span>
</div>
  

#


<div class="phpcode"><span class="html">
For those who use right-to-left languages such as Arabic, Hebrew, etc., it&apos;s worth mentioning that ltrim() (which stands for left trim) &amp; rtrim() (which stands for right trim) DO NOT work contextually. The nomenclature is rather semantically incorrect. So in an RTL script, ltrim() will trim text from the right direction (i.e. beginning of RTL strings), and rtrim() will trim text from the left direction (i.e. end of RTL strings).</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ltrim.php)

**[To root](/README.md)**