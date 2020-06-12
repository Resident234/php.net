# class_parents




<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">foo </span><span class="keyword">{}<br>class </span><span class="default">bar </span><span class="keyword">extends </span><span class="default">foo </span><span class="keyword">{}<br>class </span><span class="default">baz </span><span class="keyword">extends </span><span class="default">bar </span><span class="keyword">{}<br><br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">class_parents</span><span class="keyword">(new </span><span class="default">baz</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>Will output:<br>Array<br>(<br>&#xA0; &#xA0; [bar] =&gt; bar<br>&#xA0; &#xA0; [foo] =&gt; foo<br>)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.class-parents.php)

**[⬆ to root](/)**