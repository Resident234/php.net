# imagepng




<div class="phpcode"><span class="html">
The name &quot;quality&quot; for the compression parameter is quite misleading, as png compression is always lossless. The trade off is between speed and filesize, it cannot affect quality.<br><br>Here&apos;s something I found at stackoverflow; I haven&apos;t checked it, but if it is correct it should definitely included in the documentation:<br><br>---<br>from php source (gd.h):<br><br>/* 2.0.12: Compression level: 0-9 or -1, where 0 is NO COMPRESSION at all,<br>* 1 is FASTEST but produces larger files, 9 provides the best<br>* compression (smallest files) but takes a long time to compress, and<br>* -1 selects the default compiled into the zlib library.<br>*/<br>Conclusion: Based on the Zlib manual (<a href="http://www.zlib.net/manual.html" rel="nofollow" target="_blank">http://www.zlib.net/manual.html</a>) the default compression level is set to 6.<br>---<br><br>Regarding suggestions to rescale the 0-99 quality range of jpeg into the 0-9 range of png, note that for jpeg 99 is minimum compression (maximum quality) while for png 9 is maximum compression (quality doesn&apos;t change).</span>
</div>
  

#


<div class="phpcode"><span class="html">
&quot;Tip: As with anything that outputs its result directly to the browser, you can use the output-control functions (<a href="http://www.php.net/manual/en/ref.outcontrol.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/ref.outcontrol.php</a>) to capture the output of this function, and save it in a string (for example).&quot;<br><br>ob_start();<br>imagepng($image);<br>$image_data = ob_get_contents();<br>ob_end_clean();<br><br>And now you can save $image_data to a database, for example, instead of first writing it to file and then reading the data from it. Just don&apos;t forget to use mysql_escape_string...</span>
</div>
  

#


<div class="phpcode"><span class="html">
I just lost about 4 hours on a really stupid problem. My images on the local server were somehow broken and therefore did not display in the browsers. After much looking around and testing, including re-installing apache on my computer a couple of times, I traced the problem to an included file. <br>No the problem was not a whitespace, but the UTF BOM encoding character at the begining of one of my inluded files...<br>So beware of your included files!<br>Make sure they are not encoded in UTF or otherwise in UTF without BOM.<br>Hope it save someone&apos;s time.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be careful when using a variable for the file name.<br>PHP behavior with $filename differs when switching to PHP5.4 : PHP5.3 will use $filename=&apos;&apos; the same way as $filename=NULL (e.g. no warning)<br><span class="default">&lt;?php<br>$im </span><span class="keyword">= </span><span class="default">imagecreatetruecolor</span><span class="keyword">(</span><span class="default">10</span><span class="keyword">,</span><span class="default">10</span><span class="keyword">);<br></span><span class="default">imagepng</span><span class="keyword">(</span><span class="default">$im</span><span class="keyword">,</span><span class="string">&apos;&apos;</span><span class="keyword">,</span><span class="default">9</span><span class="keyword">); </span><span class="comment"># Warning: imagepng(): Filename cannot be empty<br></span><span class="default">imagepng</span><span class="keyword">(</span><span class="default">$im</span><span class="keyword">,</span><span class="default">NULL</span><span class="keyword">,</span><span class="default">9</span><span class="keyword">); </span><span class="comment"># works as expected<br></span><span class="default">imagedestroy</span><span class="keyword">(</span><span class="default">$im</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagepng.php)

**[⬆ to root](/)**