# namespace keyword and __NAMESPACE__ constant




<div class="phpcode"><span class="html">
Just in case you wonder what the practical use of the namespace keyword is...
<br>
<br>It can explicitly refer to classes from the current namespace regardless of possibly &quot;use&quot;d classes with the same name from other namespaces. However, this does not apply for functions.
<br>
<br>Example:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">namespace </span><span class="default">foo</span><span class="keyword">;
<br>class </span><span class="default">Xyz </span><span class="keyword">{}
<br>function </span><span class="default">abc </span><span class="keyword">() {}
<br></span><span class="default">?&gt;
<br></span>
<br><span class="default">&lt;?php
<br></span><span class="keyword">namespace </span><span class="default">bar</span><span class="keyword">;
<br>class </span><span class="default">Xyz </span><span class="keyword">{}
<br>function </span><span class="default">abc </span><span class="keyword">() {}
<br></span><span class="default">?&gt;
<br></span>
<br><span class="default">&lt;?php
<br></span><span class="keyword">namespace </span><span class="default">bar</span><span class="keyword">;
<br>use </span><span class="default">foo</span><span class="keyword">\</span><span class="default">Xyz</span><span class="keyword">;
<br>use </span><span class="default">foo</span><span class="keyword">\</span><span class="default">abc</span><span class="keyword">;
<br>new </span><span class="default">Xyz</span><span class="keyword">(); </span><span class="comment">// instantiates \foo\Xyz
<br></span><span class="keyword">new namespace\</span><span class="default">Xyz</span><span class="keyword">(); </span><span class="comment">// instantiates \bar\Xyz
<br></span><span class="default">abc</span><span class="keyword">(); </span><span class="comment">// invokes \bar\abc regardless of the second use statement
<br></span><span class="keyword">\</span><span class="default">foo</span><span class="keyword">\</span><span class="default">abc</span><span class="keyword">(); </span><span class="comment">// it has to be invoked using the fully qualified name
<br></span><span class="default">?&gt;
<br></span>
<br>Hope, this can save someone from some trouble.
<br>
<br>Best regards.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.nsconstants.php)

**[To root](/)**