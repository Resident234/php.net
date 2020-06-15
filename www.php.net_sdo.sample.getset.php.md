# Setting and Getting Property Values




<div class="phpcode"><span class="html">
You can also use curly brackets to evaluate property names (e.g., to access a property name containing special [non-standard] characters without first needing to assign the name to a variable).<br><br><span class="default">&lt;?php<br><br>$object </span><span class="keyword">= new </span><span class="default">StdClass</span><span class="keyword">();<br><br></span><span class="comment">// standard access using variable<br></span><span class="default">$nonStandardPropertyName </span><span class="keyword">= </span><span class="string">&apos;/a/path?for=example&apos;</span><span class="keyword">;<br></span><span class="default">$object</span><span class="keyword">-&gt;</span><span class="default">$nonStandardPropertyName </span><span class="keyword">= </span><span class="string">&apos;access using a variable&apos;</span><span class="keyword">;<br><br></span><span class="comment">// alternative access using curly braces<br></span><span class="default">$object</span><span class="keyword">-&gt;{</span><span class="string">&apos;/a/path?for=example&apos;</span><span class="keyword">} = </span><span class="string">&apos;set using curly braces&apos;</span><span class="keyword">;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
See also: $this pseudo-variable, <a href="http://php.net/manual/en/language.oop5.basic.php" rel="nofollow" target="_blank">http://php.net/manual/en/language.oop5.basic.php</a><br><br>In addition, when inside a class method function, indirect access to object properties also works:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; $name </span><span class="keyword">= </span><span class="string">&apos;departments&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">$departments </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">$name</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">$name </span><span class="keyword">= </span><span class="default">$different</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/sdo.sample.getset.php)

**[To root](/README.md)**