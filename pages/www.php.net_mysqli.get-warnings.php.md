# mysqli::get_warnings




<div class="phpcode"><span class="html">
With a bit of rooting about with reflection, I spotted that the mysqli_warning class has a next() function, so I tried calling it and it does indeed progress through the available warnings! Following on from my earlier example:
<br>
<br><span class="default">&lt;?php
<br>$r </span><span class="keyword">= </span><span class="default">mysqli_query</span><span class="keyword">(</span><span class="default">$db</span><span class="keyword">, </span><span class="string">&quot;INSERT INTO blah SET z = &apos;1&apos;&quot;</span><span class="keyword">);
<br></span><span class="default">$j </span><span class="keyword">= </span><span class="default">mysqli_warning_count</span><span class="keyword">(</span><span class="default">$db</span><span class="keyword">);
<br>if (</span><span class="default">$j </span><span class="keyword">&gt; </span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$e </span><span class="keyword">= </span><span class="default">mysqli_get_warnings</span><span class="keyword">(</span><span class="default">$db</span><span class="keyword">);
<br>&#xA0; &#xA0; for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">$j</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$e</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">next</span><span class="keyword">();
<br>&#xA0; &#xA0; }
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>There is a simple way of traversing the warnings:
<br>
<br><span class="default">&lt;?php
<br>$r </span><span class="keyword">= </span><span class="default">mysqli_query</span><span class="keyword">(</span><span class="default">$db</span><span class="keyword">, </span><span class="string">&quot;INSERT INTO blah SET z = &apos;1&apos;&quot;</span><span class="keyword">);
<br>if (</span><span class="default">mysqli_warning_count</span><span class="keyword">(</span><span class="default">$db</span><span class="keyword">)) {
<br>&#xA0;&#xA0; </span><span class="default">$e </span><span class="keyword">= </span><span class="default">mysqli_get_warnings</span><span class="keyword">(</span><span class="default">$db</span><span class="keyword">);
<br>&#xA0;&#xA0; do {
<br>&#xA0; &#xA0; &#xA0;&#xA0; echo </span><span class="string">&quot;Warning: </span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">errno</span><span class="string">: </span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">message</span><span class="string">\n&quot;</span><span class="keyword">;
<br>&#xA0;&#xA0; } while (</span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">next</span><span class="keyword">());
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.get-warnings.php)

**[To root](/README.md)**