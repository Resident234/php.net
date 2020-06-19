# zip_open




<div class="phpcode"><span class="html">
Note that the Zip functions return an integer error number in the event of error. So:<br><br><span class="default">&lt;?php<br>$zip </span><span class="keyword">= </span><span class="default">zip_open</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">);<br><br>if (</span><span class="default">$zip</span><span class="keyword">) {<br></span><span class="default">?&gt;<br></span><br>is incorrect. Instead use:<br><br><span class="default">&lt;?php<br>$zip </span><span class="keyword">= </span><span class="default">zip_open</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">);<br><br>if (</span><span class="default">is_resource</span><span class="keyword">(</span><span class="default">$zip</span><span class="keyword">)) {<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.zip-open.php)

**[To root](/README.md)**