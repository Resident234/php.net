# preg_quote




<div class="phpcode"><span class="html">
Wondering why your preg_replace fails, even if you have used preg_quote?<br><br>Try adding the delimiter / - preg_quote($string, &apos;/&apos;);</span>
</div>
  

#


<div class="phpcode"><span class="html">
To escape characters with special meaning, like: .-[]() and so on, use \Q and \E.<br><br>For example:<br><br><span class="default">&lt;?php </span><span class="keyword">echo ( </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/^&apos;</span><span class="keyword">.( </span><span class="default">$myvar </span><span class="keyword">= </span><span class="string">&apos;te.t&apos; </span><span class="keyword">).</span><span class="string">&apos;$/i&apos;</span><span class="keyword">, </span><span class="string">&apos;test&apos;</span><span class="keyword">) ? </span><span class="string">&apos;match&apos; </span><span class="keyword">: </span><span class="string">&apos;nomatch&apos; </span><span class="keyword">); </span><span class="default">?&gt;<br></span><br>Will result in: match<br><br>But:<br><br><span class="default">&lt;?php </span><span class="keyword">echo ( </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/^\Q&apos;</span><span class="keyword">.( </span><span class="default">$myvar </span><span class="keyword">= </span><span class="string">&apos;te.t&apos; </span><span class="keyword">).</span><span class="string">&apos;\E$/i&apos;</span><span class="keyword">, </span><span class="string">&apos;test&apos;</span><span class="keyword">) ? </span><span class="string">&apos;match&apos; </span><span class="keyword">: </span><span class="string">&apos;nomatch&apos; </span><span class="keyword">); </span><span class="default">?&gt;<br></span><br>Will result in: nomatch</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.preg-quote.php)

**[To root](/README.md)**