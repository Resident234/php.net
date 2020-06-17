# mcrypt_decrypt




<div class="phpcode"><span class="html">
It appears that mcrypt_decrypt pads the *RETURN STRING* with nulls (&apos;\0&apos;) to fill out to n * blocksize.&#xA0; For old C-programmers, like myself, it is easy to believe the string ends at the first null.&#xA0; In PHP it does not:
<br>
<br>&#xA0; &#xA0; strlen(&quot;abc\0\0&quot;) returns 5 and *NOT* 3
<br>&#xA0; &#xA0; strcmp(&quot;abc&quot;, &quot;abc\0\0&quot;) returns -2 and *NOT* 0
<br>
<br>I learned this lesson painfully when I passed a string returned from mycrypt_decrypt into a NuSoap message, which happily passed the nulls along to the receiver, who couldn&apos;t figure out what I was talking about.
<br>
<br>My solution was:
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0; $retval </span><span class="keyword">= </span><span class="default">mcrypt_decrypt</span><span class="keyword">( ...</span><span class="default">etc </span><span class="keyword">...);
<br>&#xA0; &#xA0; </span><span class="default">$retval </span><span class="keyword">= </span><span class="default">rtrim</span><span class="keyword">(</span><span class="default">$retval</span><span class="keyword">, </span><span class="string">&quot;\0&quot;</span><span class="keyword">);&#xA0; &#xA0;&#xA0; </span><span class="comment">// trim ONLY the nulls at the END
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.mcrypt-decrypt.php)

**[To root](/README.md)**