# gzencode




<div class="phpcode"><span class="html">
Had some trouble finding the correct way to send a Content-Length header with HTTP compression.<br>The pitch is to use gzencode (not gzdeflaten not gzcompress).<br><br><span class="default">&lt;?php<br><br></span><span class="comment">// disable ZLIB ouput compression<br></span><span class="default">ini_set</span><span class="keyword">(</span><span class="string">&apos;zlib.output_compression&apos;</span><span class="keyword">,</span><span class="string">&apos;Off&apos;</span><span class="keyword">);<br><br></span><span class="comment">// compress data<br></span><span class="default">$gzipoutput </span><span class="keyword">= </span><span class="default">gzencode</span><span class="keyword">(</span><span class="default">$output</span><span class="keyword">,</span><span class="default">6</span><span class="keyword">);<br><br></span><span class="comment">// various headers, those with # are mandatory<br></span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Content-Type: application/x-download&apos;</span><span class="keyword">);<br></span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Content-Encoding: gzip&apos;</span><span class="keyword">); </span><span class="comment">#<br></span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Content-Length: &apos;</span><span class="keyword">.</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$gzipoutput</span><span class="keyword">)); </span><span class="comment">#<br></span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Content-Disposition: attachment; filename=&quot;myfile.name&quot;&apos;</span><span class="keyword">);<br></span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Cache-Control: no-cache, no-store, max-age=0, must-revalidate&apos;</span><span class="keyword">);<br></span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Pragma: no-cache&apos;</span><span class="keyword">);<br><br></span><span class="comment">// output data<br></span><span class="keyword">echo </span><span class="default">$gzipoutput</span><span class="keyword">;<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.gzencode.php)

**[⬆ to root](/)**