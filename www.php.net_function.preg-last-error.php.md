# preg_last_error




<div class="phpcode"><span class="html">
In PHP 5.5 and above, getting the error message is as simple as:<br><br><span class="default">&lt;?php<br></span><span class="keyword">echo </span><span class="default">array_flip</span><span class="keyword">(</span><span class="default">get_defined_constants</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">)[</span><span class="string">&apos;pcre&apos;</span><span class="keyword">])[</span><span class="default">preg_last_error</span><span class="keyword">()];</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is a more advanced function to convert an error code to text:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">preg_errtxt</span><span class="keyword">(</span><span class="default">$errcode</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; static </span><span class="default">$errtext</span><span class="keyword">;<br><br>&#xA0; &#xA0; if (!isset(</span><span class="default">$errtxt</span><span class="keyword">))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$errtext </span><span class="keyword">= array();<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$constants </span><span class="keyword">= </span><span class="default">get_defined_constants</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$constants</span><span class="keyword">[</span><span class="string">&apos;pcre&apos;</span><span class="keyword">] as </span><span class="default">$c </span><span class="keyword">=&gt; </span><span class="default">$n</span><span class="keyword">) if (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/_ERROR$/&apos;</span><span class="keyword">, </span><span class="default">$c</span><span class="keyword">)) </span><span class="default">$errtext</span><span class="keyword">[</span><span class="default">$n</span><span class="keyword">] = </span><span class="default">$c</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; return </span><span class="default">array_key_exists</span><span class="keyword">(</span><span class="default">$errcode</span><span class="keyword">, </span><span class="default">$errtext</span><span class="keyword">)? </span><span class="default">$errtext</span><span class="keyword">[</span><span class="default">$errcode</span><span class="keyword">] : </span><span class="default">NULL</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.preg-last-error.php)

**[⬆ to root](/)**