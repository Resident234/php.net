# ob_gzhandler




<div class="phpcode"><span class="html">
I&apos;ve just spent 5 hours fighting a bug in my application and outcome is:<br><br><span class="default">&lt;?php<br></span><span class="comment">// do not use<br></span><span class="default">ob_start</span><span class="keyword">(</span><span class="string">&quot;ob_gzhandler&quot;</span><span class="keyword">);<br></span><span class="comment">// along with<br></span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;HTTP/1.1 304 Not Modified&apos;</span><span class="keyword">);<br><br></span><span class="comment">// or in the end use<br></span><span class="default">ob_end_clean</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br>W3C Standart requires response body to be empty if 304 header set. With compression on it will at least contain a gzip stream header even if your output is completely empty! <br><br>This affects firefox (current ver.3.6.3) in a very subtle way: one of the requests after the one that gets 304 with not empty body gets it response prepended with contents of that body. In my case it was a css file and styles was not rendered at all, which made problem show up.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ob-gzhandler.php)

**[⬆ to root](/)**