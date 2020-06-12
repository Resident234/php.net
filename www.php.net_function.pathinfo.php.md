# pathinfo




<div class="phpcode"><span class="html">
Simple example of pathinfo and array destructuring in PHP 7:<br><span class="default">&lt;?php<br></span><span class="keyword">[ </span><span class="string">&apos;basename&apos; </span><span class="keyword">=&gt; </span><span class="default">$basename</span><span class="keyword">, </span><span class="string">&apos;dirname&apos; </span><span class="keyword">=&gt; </span><span class="default">$dirname </span><span class="keyword">] = </span><span class="default">pathinfo</span><span class="keyword">(</span><span class="string">&apos;/www/htdocs/inc/lib.inc.php&apos;</span><span class="keyword">);<br><br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$basename</span><span class="keyword">, </span><span class="default">$dirname</span><span class="keyword">);<br><br></span><span class="comment">// result:<br>// string(11) &quot;lib.inc.php&quot;<br>// string(15) &quot;/www/htdocs/inc&quot;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note:<br><br>pathinfo() is locale aware, so for it to parse a path containing multibyte characters correctly, the matching locale must be set using the setlocale() function. <br><br>Reality:<br>var_dump(pathinfo(&apos;&#x4E2D;&#x56FD;&#x4EBA;2016.xls&apos;));<br>exit();<br>array(4) { &apos;dirname&apos; =&gt; string(1) &quot;.&quot; &apos;basename&apos; =&gt; string(8) &quot;2016.xls&quot; &apos;extension&apos; =&gt; string(3) &quot;xls&quot; &apos;filename&apos; =&gt; string(4) &quot;2016&quot; } <br><br>Expect(Solve):<br>setlocale(LC_ALL, &apos;zh_CN.UTF-8&apos;);<br>var_dump(pathinfo(&apos;&#x4E2D;&#x56FD;&#x4EBA;2016.xls&apos;));<br>exit();<br>array(4) { &apos;dirname&apos; =&gt; string(1) &quot;.&quot; &apos;basename&apos; =&gt; string(17) &quot;&#x4E2D;&#x56FD;&#x4EBA;2016.xls&quot; &apos;extension&apos; =&gt; string(3) &quot;xls&quot; &apos;filename&apos; =&gt; string(13) &quot;&#x4E2D;&#x56FD;&#x4EBA;2016&quot; }</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.pathinfo.php)

**[⬆ to root](/)**