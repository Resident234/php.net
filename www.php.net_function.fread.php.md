# fread




<div class="phpcode"><span class="html">
I couldn&apos;t get some of the previous resume scripts to work with Free Download Manager or Firefox.&#xA0; I did some clean up and modified the code a little.<br><br>Changes:<br>1. Added a Flag to specify if you want download to be resumable or not<br>2. Some error checking and data cleanup for invalid/multiple ranges based on <a href="http://tools.ietf.org/id/draft-ietf-http-range-retrieval-00.txt" rel="nofollow" target="_blank">http://tools.ietf.org/id/draft-ietf-http-range-retrieval-00.txt</a><br>3. Always calculate a $seek_end even though the range specification says it could be empty... eg: bytes 500-/1234<br>4. Removed some Cache headers that didn&apos;t seem to be needed. (add back if you have problems)<br>5. Only send partial content header if downloading a piece of the file (IE workaround)<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">dl_file_resumable</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">, </span><span class="default">$is_resume</span><span class="keyword">=</span><span class="default">TRUE</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="comment">//First, see if the file exists<br>&#xA0; &#xA0; </span><span class="keyword">if (!</span><span class="default">is_file</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; die(</span><span class="string">&quot;&lt;b&gt;404 File not found!&lt;/b&gt;&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment">//Gather relevent info about file<br>&#xA0; &#xA0; </span><span class="default">$size </span><span class="keyword">= </span><span class="default">filesize</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$fileinfo </span><span class="keyword">= </span><span class="default">pathinfo</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="comment">//workaround for IE filename bug with multiple periods / multiple dots in filename<br>&#xA0; &#xA0; //that adds square brackets to filename - eg. setup.abc.exe becomes setup[1].abc.exe<br>&#xA0; &#xA0; </span><span class="default">$filename </span><span class="keyword">= (</span><span class="default">strstr</span><span class="keyword">(</span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;HTTP_USER_AGENT&apos;</span><span class="keyword">], </span><span class="string">&apos;MSIE&apos;</span><span class="keyword">)) ?<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/\./&apos;</span><span class="keyword">, </span><span class="string">&apos;%2e&apos;</span><span class="keyword">, </span><span class="default">$fileinfo</span><span class="keyword">[</span><span class="string">&apos;basename&apos;</span><span class="keyword">], </span><span class="default">substr_count</span><span class="keyword">(</span><span class="default">$fileinfo</span><span class="keyword">[</span><span class="string">&apos;basename&apos;</span><span class="keyword">], </span><span class="string">&apos;.&apos;</span><span class="keyword">) - </span><span class="default">1</span><span class="keyword">) :<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$fileinfo</span><span class="keyword">[</span><span class="string">&apos;basename&apos;</span><span class="keyword">];<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">$file_extension </span><span class="keyword">= </span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$path_info</span><span class="keyword">[</span><span class="string">&apos;extension&apos;</span><span class="keyword">]);<br><br>&#xA0; &#xA0; </span><span class="comment">//This will set the Content-Type to the appropriate setting for the file<br>&#xA0; &#xA0; </span><span class="keyword">switch(</span><span class="default">$file_extension</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="string">&apos;exe&apos;</span><span class="keyword">: </span><span class="default">$ctype</span><span class="keyword">=</span><span class="string">&apos;application/octet-stream&apos;</span><span class="keyword">; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="string">&apos;zip&apos;</span><span class="keyword">: </span><span class="default">$ctype</span><span class="keyword">=</span><span class="string">&apos;application/zip&apos;</span><span class="keyword">; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="string">&apos;mp3&apos;</span><span class="keyword">: </span><span class="default">$ctype</span><span class="keyword">=</span><span class="string">&apos;audio/mpeg&apos;</span><span class="keyword">; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="string">&apos;mpg&apos;</span><span class="keyword">: </span><span class="default">$ctype</span><span class="keyword">=</span><span class="string">&apos;video/mpeg&apos;</span><span class="keyword">; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="string">&apos;avi&apos;</span><span class="keyword">: </span><span class="default">$ctype</span><span class="keyword">=</span><span class="string">&apos;video/x-msvideo&apos;</span><span class="keyword">; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; default:&#xA0; &#xA0; </span><span class="default">$ctype</span><span class="keyword">=</span><span class="string">&apos;application/force-download&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment">//check if http_range is sent by browser (or download manager)<br>&#xA0; &#xA0; </span><span class="keyword">if(</span><span class="default">$is_resume </span><span class="keyword">&amp;&amp; isset(</span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;HTTP_RANGE&apos;</span><span class="keyword">]))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; list(</span><span class="default">$size_unit</span><span class="keyword">, </span><span class="default">$range_orig</span><span class="keyword">) = </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos;=&apos;</span><span class="keyword">, </span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;HTTP_RANGE&apos;</span><span class="keyword">], </span><span class="default">2</span><span class="keyword">);<br><br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$size_unit </span><span class="keyword">== </span><span class="string">&apos;bytes&apos;</span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//multiple ranges could be specified at the same time, but for simplicity only serve the first range<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //<a href="http://tools.ietf.org/id/draft-ietf-http-range-retrieval-00.txt" rel="nofollow" target="_blank">http://tools.ietf.org/id/draft-ietf-http-range-retrieval-00.txt</a><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">list(</span><span class="default">$range</span><span class="keyword">, </span><span class="default">$extra_ranges</span><span class="keyword">) = </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos;,&apos;</span><span class="keyword">, </span><span class="default">$range_orig</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; else<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$range </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; else<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$range </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment">//figure out download piece from range (if set)<br>&#xA0; &#xA0; </span><span class="keyword">list(</span><span class="default">$seek_start</span><span class="keyword">, </span><span class="default">$seek_end</span><span class="keyword">) = </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos;-&apos;</span><span class="keyword">, </span><span class="default">$range</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="comment">//set start and end based on range (if set), else set defaults<br>&#xA0; &#xA0; //also check for invalid ranges.<br>&#xA0; &#xA0; </span><span class="default">$seek_end </span><span class="keyword">= (empty(</span><span class="default">$seek_end</span><span class="keyword">)) ? (</span><span class="default">$size </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">) : </span><span class="default">min</span><span class="keyword">(</span><span class="default">abs</span><span class="keyword">(</span><span class="default">intval</span><span class="keyword">(</span><span class="default">$seek_end</span><span class="keyword">)),(</span><span class="default">$size </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">));<br>&#xA0; &#xA0; </span><span class="default">$seek_start </span><span class="keyword">= (empty(</span><span class="default">$seek_start</span><span class="keyword">) || </span><span class="default">$seek_end </span><span class="keyword">&lt; </span><span class="default">abs</span><span class="keyword">(</span><span class="default">intval</span><span class="keyword">(</span><span class="default">$seek_start</span><span class="keyword">))) ? </span><span class="default">0 </span><span class="keyword">: </span><span class="default">max</span><span class="keyword">(</span><span class="default">abs</span><span class="keyword">(</span><span class="default">intval</span><span class="keyword">(</span><span class="default">$seek_start</span><span class="keyword">)),</span><span class="default">0</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="comment">//add headers if resumable<br>&#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">$is_resume</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//Only send partial content header if downloading a piece of the file (IE workaround)<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">$seek_start </span><span class="keyword">&gt; </span><span class="default">0 </span><span class="keyword">|| </span><span class="default">$seek_end </span><span class="keyword">&lt; (</span><span class="default">$size </span><span class="keyword">- </span><span class="default">1</span><span class="keyword">))<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;HTTP/1.1 206 Partial Content&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Accept-Ranges: bytes&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Content-Range: bytes &apos;</span><span class="keyword">.</span><span class="default">$seek_start</span><span class="keyword">.</span><span class="string">&apos;-&apos;</span><span class="keyword">.</span><span class="default">$seek_end</span><span class="keyword">.</span><span class="string">&apos;/&apos;</span><span class="keyword">.</span><span class="default">$size</span><span class="keyword">);<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="comment">//headers for IE Bugs (is this necessary?)<br>&#xA0; &#xA0; //header(&quot;Cache-Control: cache, must-revalidate&quot;);&#xA0;&#xA0; <br>&#xA0; &#xA0; //header(&quot;Pragma: public&quot;);<br><br>&#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Content-Type: &apos; </span><span class="keyword">. </span><span class="default">$ctype</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Content-Disposition: attachment; filename=&quot;&apos; </span><span class="keyword">. </span><span class="default">$filename </span><span class="keyword">. </span><span class="string">&apos;&quot;&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">header</span><span class="keyword">(</span><span class="string">&apos;Content-Length: &apos;</span><span class="keyword">.(</span><span class="default">$seek_end </span><span class="keyword">- </span><span class="default">$seek_start </span><span class="keyword">+ </span><span class="default">1</span><span class="keyword">));<br><br>&#xA0; &#xA0; </span><span class="comment">//open the file<br>&#xA0; &#xA0; </span><span class="default">$fp </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">, </span><span class="string">&apos;rb&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="comment">//seek to start of missing part<br>&#xA0; &#xA0; </span><span class="default">fseek</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">$seek_start</span><span class="keyword">);<br><br>&#xA0; &#xA0; </span><span class="comment">//start buffered download<br>&#xA0; &#xA0; </span><span class="keyword">while(!</span><span class="default">feof</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//reset time limit for big files<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">set_time_limit</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; print(</span><span class="default">fread</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">1024</span><span class="keyword">*</span><span class="default">8</span><span class="keyword">));<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">flush</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">ob_flush</span><span class="keyword">();<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);<br>&#xA0; &#xA0; exit;<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
This is an hack I&apos;ve done to download remote files with HTTP resume support. This is useful if you want to write a download script that fetches files remotely and then sends them to the user, adding support to download managers (I tested it on wget). To do that you should also use a &quot;remote_filesize&quot; function that you can easily write/find.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">readfile_chunked_remote</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">, </span><span class="default">$seek </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">, </span><span class="default">$retbytes </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">, </span><span class="default">$timeout </span><span class="keyword">= </span><span class="default">3</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">set_time_limit</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$defaultchunksize </span><span class="keyword">= </span><span class="default">1024</span><span class="keyword">*</span><span class="default">1024</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$chunksize </span><span class="keyword">= </span><span class="default">$defaultchunksize</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$buffer </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$cnt </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$remotereadfile </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; if (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/[a-zA-Z]+:\/\//&apos;</span><span class="keyword">, </span><span class="default">$filename</span><span class="keyword">))
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$remotereadfile </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; </span><span class="default">$handle </span><span class="keyword">= @</span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">, </span><span class="string">&apos;rb&apos;</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; if (</span><span class="default">$handle </span><span class="keyword">=== </span><span class="default">false</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; </span><span class="default">stream_set_timeout</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">, </span><span class="default">$timeout</span><span class="keyword">);
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; if (</span><span class="default">$seek </span><span class="keyword">!= </span><span class="default">0 </span><span class="keyword">&amp;&amp; !</span><span class="default">$remotereadfile</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fseek</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">, </span><span class="default">$seek</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; while (!</span><span class="default">feof</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">)) {
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$remotereadfile </span><span class="keyword">&amp;&amp; </span><span class="default">$seek </span><span class="keyword">!= </span><span class="default">0 </span><span class="keyword">&amp;&amp; </span><span class="default">$cnt</span><span class="keyword">+</span><span class="default">$chunksize </span><span class="keyword">&gt; </span><span class="default">$seek</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$chunksize </span><span class="keyword">= </span><span class="default">$seek</span><span class="keyword">-</span><span class="default">$cnt</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; else
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$chunksize </span><span class="keyword">= </span><span class="default">$defaultchunksize</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$buffer </span><span class="keyword">= @</span><span class="default">fread</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">, </span><span class="default">$chunksize</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$retbytes </span><span class="keyword">|| (</span><span class="default">$remotereadfile </span><span class="keyword">&amp;&amp; </span><span class="default">$seek </span><span class="keyword">!= </span><span class="default">0</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$cnt </span><span class="keyword">+= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$buffer</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">$remotereadfile </span><span class="keyword">|| (</span><span class="default">$remotereadfile </span><span class="keyword">&amp;&amp; </span><span class="default">$cnt </span><span class="keyword">&gt; </span><span class="default">$seek</span><span class="keyword">))
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">$buffer</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">ob_flush</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">flush</span><span class="keyword">();
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; </span><span class="default">$info </span><span class="keyword">= </span><span class="default">stream_get_meta_data</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; </span><span class="default">$status </span><span class="keyword">= </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; if (</span><span class="default">$info</span><span class="keyword">[</span><span class="string">&apos;timed_out&apos;</span><span class="keyword">])
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; if (</span><span class="default">$retbytes </span><span class="keyword">&amp;&amp; </span><span class="default">$status</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$cnt</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; return </span><span class="default">$status</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I had a fread script that hanged forever (from php manual):<br><br><span class="default">&lt;?php<br>$fp </span><span class="keyword">= </span><span class="default">fsockopen</span><span class="keyword">(</span><span class="string">&quot;example.host.com&quot;</span><span class="keyword">, </span><span class="default">80</span><span class="keyword">);<br>if (!</span><span class="default">$fp</span><span class="keyword">) {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;</span><span class="default">$errstr</span><span class="string"> (</span><span class="default">$errno</span><span class="string">)&lt;br /&gt;\n&quot;</span><span class="keyword">;<br>} else {<br>&#xA0; &#xA0; </span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="string">&quot;Data sent by socket&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$content </span><span class="keyword">= </span><span class="string">&quot;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; while (!</span><span class="default">feof</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">)) {&#xA0; </span><span class="comment">//This looped forever<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$content </span><span class="keyword">.= </span><span class="default">fread</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">1024</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);<br>&#xA0; &#xA0; echo </span><span class="default">$content</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>The problem is that sometimes end of streaming is not marked by EOF nor a fixed mark, that&apos;s why this looped forever. This caused me a lot of headaches...<br>I solved it using the stream_get_meta_data function and a break statement as the following shows:<br><br><span class="default">&lt;?php<br>$fp </span><span class="keyword">= </span><span class="default">fsockopen</span><span class="keyword">(</span><span class="string">&quot;example.host.com&quot;</span><span class="keyword">, </span><span class="default">80</span><span class="keyword">);<br>if (!</span><span class="default">$fp</span><span class="keyword">) {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;</span><span class="default">$errstr</span><span class="string"> (</span><span class="default">$errno</span><span class="string">)&lt;br /&gt;\n&quot;</span><span class="keyword">;<br>} else {<br>&#xA0; &#xA0; </span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="string">&quot;Data sent by socket&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$content </span><span class="keyword">= </span><span class="string">&quot;&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; while (!</span><span class="default">feof</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">)) {&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$content </span><span class="keyword">.= </span><span class="default">fread</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">1024</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$stream_meta_data </span><span class="keyword">= </span><span class="default">stream_get_meta_data</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">); </span><span class="comment">//Added line<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">if(</span><span class="default">$stream_meta_data</span><span class="keyword">[</span><span class="string">&apos;unread_bytes&apos;</span><span class="keyword">] &lt;= </span><span class="default">0</span><span class="keyword">) break; </span><span class="comment">//Added line<br>&#xA0; &#xA0; </span><span class="keyword">}<br>&#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);<br>&#xA0; &#xA0; echo </span><span class="default">$content</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>Hope this will save a lot of headaches to someone.<br><br>(Greetings, from La Paz-Bolivia)</span>
</div>
  

#


<div class="phpcode"><span class="html">
For anyone still trying to write an effective file downloader function/script, the work has been done for you in all the major servers including Apache &amp; nginx.<br><br>Using the X-Sendfile header, you can do the following:<br><br>if ($user-&gt;isLoggedIn())<br>{<br>&#xA0; &#xA0; header(&quot;X-Sendfile: $path_to_somefile_private&quot;);<br>&#xA0; &#xA0; header(&quot;Content-Type: application/octet-stream&quot;);<br>&#xA0; &#xA0; header(&quot;Content-Disposition: attachment; filename=\&quot;$somefile\&quot;&quot;);<br>}<br><br>Apache will serve the file for you while NOT revealing your private file path! Pretty nice. This works on all browsers/download managers and saves a lot of resources.<br><br>Documentation:<br>Apache module: <a href="https://tn123.org/mod_xsendfile/" rel="nofollow" target="_blank">https://tn123.org/mod_xsendfile/</a><br>Nginx: <a href="http://wiki.nginx.org/XSendfile" rel="nofollow" target="_blank">http://wiki.nginx.org/XSendfile</a><br>Lighttpd: <a href="http://blog.lighttpd.net/articles/2006/07/02/x-sendfile/" rel="nofollow" target="_blank">http://blog.lighttpd.net/articles/2006/07/02/x-sendfile/</a><br><br>Hopefully this will save you many hours of work.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.fread.php)

**[To root](/README.md)**