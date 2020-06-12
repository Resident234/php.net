# finfo_open




<div class="phpcode"><span class="html">
I am running Windows 7 with Apache.&#xA0; It took hours to figure out why it was not working.<br><br>First, enable the php_fileinfo.dll extension in you php.ini. You&apos;ll also need the four magic files that are found in the following library:<br><br><a href="http://sourceforge.net/projects/gnuwin32/files/file/4.23/file-4.23-bin.zip/download" rel="nofollow" target="_blank">http://sourceforge.net/projects/gnuwin32/files/file/4.23/file-4.23-bin.zip/download</a><br><br>An environment variable or a direct path to the file named &quot;magic&quot; is necessary, without any extension.&#xA0; <br><br>Then, make sure that xdebug is either turned off or set the ini error_reporting to not display notices or warnings for the script.<br><br>Hope this saves someone a few hours of frustration!</span>
</div>
  

#


<div class="phpcode"><span class="html">
For most common image files:<br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">minimime</span><span class="keyword">(</span><span class="default">$fname</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$fh</span><span class="keyword">=</span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$fname</span><span class="keyword">,</span><span class="string">&apos;rb&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; if (</span><span class="default">$fh</span><span class="keyword">) { <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$bytes6</span><span class="keyword">=</span><span class="default">fread</span><span class="keyword">(</span><span class="default">$fh</span><span class="keyword">,</span><span class="default">6</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fh</span><span class="keyword">); <br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$bytes6</span><span class="keyword">===</span><span class="default">false</span><span class="keyword">) return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">substr</span><span class="keyword">(</span><span class="default">$bytes6</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">,</span><span class="default">3</span><span class="keyword">)==</span><span class="string">&quot;\xff\xd8\xff&quot;</span><span class="keyword">) return </span><span class="string">&apos;image/jpeg&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$bytes6</span><span class="keyword">==</span><span class="string">&quot;\x89PNG\x0d\x0a&quot;</span><span class="keyword">) return </span><span class="string">&apos;image/png&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$bytes6</span><span class="keyword">==</span><span class="string">&quot;GIF87a&quot; </span><span class="keyword">|| </span><span class="default">$bytes6</span><span class="keyword">==</span><span class="string">&quot;GIF89a&quot;</span><span class="keyword">) return </span><span class="string">&apos;image/gif&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="string">&apos;application/octet-stream&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.finfo-open.php)

**[To root](/README.md)**