# Delimiters




<div class="phpcode"><span class="html">
Note that bracket style opening and closing delimiters aren&apos;t a 100% problem-free solution, as they need to be escaped when they aren&apos;t in matching pairs within the expression. That mismatch can happen when they appear inside character classes [...], as most meta-characters lose their special meaning. Consider these examples:<br><br><span class="default">&lt;?php<br>&#xA0; preg_match</span><span class="keyword">(</span><span class="string">&apos;{[{]}&apos;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">); </span><span class="comment">// Warning: preg_match(): No ending matching delimiter &apos;}&apos;<br>&#xA0; </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;{[}]}&apos;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">); </span><span class="comment">// Warning: preg_match(): Unknown modifier &apos;]&apos;<br>&#xA0; </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;{[}{]}&apos;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">); </span><span class="comment">// Warning: preg_match(): Unknown modifier &apos;]&apos;<br></span><span class="default">?&gt;<br></span><br>Escaping them solves it:<br><br><span class="default">&lt;?php<br>&#xA0; preg_match</span><span class="keyword">(</span><span class="string">&apos;{[\{]}&apos;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">); </span><span class="comment">// OK<br>&#xA0; </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;{[}]}&apos;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">); </span><span class="comment">// OK<br>&#xA0; </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;{[\}\{]}&apos;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">); </span><span class="comment">// OK<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/regexp.reference.delimiters.php)

**[⬆ to root](/)**