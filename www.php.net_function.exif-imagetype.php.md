# exif_imagetype




<div class="phpcode"><span class="html">
Because I only want to check for jpeg or png from a memory string, this is my 2 functions that are quick and don&apos;t have any dependencies :<br><br><span class="default">&lt;?php<br>&#xA0; </span><span class="keyword">function </span><span class="default">is_jpeg</span><span class="keyword">(&amp;</span><span class="default">$pict</span><span class="keyword">)<br>&#xA0; {<br>&#xA0; &#xA0; return (</span><span class="default">bin2hex</span><span class="keyword">(</span><span class="default">$pict</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]) == </span><span class="string">&apos;ff&apos; </span><span class="keyword">&amp;&amp; </span><span class="default">bin2hex</span><span class="keyword">(</span><span class="default">$pict</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">]) == </span><span class="string">&apos;d8&apos;</span><span class="keyword">);<br>&#xA0; }<br><br>&#xA0; function </span><span class="default">is_png</span><span class="keyword">(&amp;</span><span class="default">$pict</span><span class="keyword">)<br>&#xA0; {<br>&#xA0; &#xA0; return (</span><span class="default">bin2hex</span><span class="keyword">(</span><span class="default">$pict</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]) == </span><span class="string">&apos;89&apos; </span><span class="keyword">&amp;&amp; </span><span class="default">$pict</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] == </span><span class="string">&apos;P&apos; </span><span class="keyword">&amp;&amp; </span><span class="default">$pict</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">] == </span><span class="string">&apos;N&apos; </span><span class="keyword">&amp;&amp; </span><span class="default">$pict</span><span class="keyword">[</span><span class="default">3</span><span class="keyword">] == </span><span class="string">&apos;G&apos;</span><span class="keyword">);<br>&#xA0; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
By trial and error, it seems that a file has to be 12 bytes or larger in order to avoid a &quot;Read error!&quot;.&#xA0; Here&apos;s a work-around to avoid an error being thrown:<br><br>// exif_imagetype throws &quot;Read error!&quot; if file is too small<br>if (filesize($uploadfile) &gt; 11)<br>&#xA0; &#xA0; $mimetype = exif_imagetype($uploadfile);<br>else<br>&#xA0; &#xA0; $mimetype = false;</span>
</div>
  

#


<div class="phpcode"><span class="html">
Windows users: If you get the fatal error &quot;Fatal error:&#xA0; Call to undefined function exif_imagetype()&quot;, and you have enabled php_exif.dll, make sure you enable php_mbstring.dll. You must put mbstring before exif in the php.ini, i.e.:<br><br>extension=php_mbstring.dll<br>extension=php_exif.dll<br><br>You can check whether this has worked by calling phpinfo() and searching for exif.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.exif-imagetype.php)

**[⬆ to root](/)**