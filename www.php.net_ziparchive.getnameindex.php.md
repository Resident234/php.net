# ZipArchive::getNameIndex




<div class="phpcode"><span class="html">
I couldn&apos;t find any how-to example for getting the filenames, so I made an easy one.
<br>
<br>Here&apos;s an example how to list all filenames from a zip-archive:
<br>
<br><span class="default">&lt;?php
<br>$zip </span><span class="keyword">= new </span><span class="default">ZipArchive</span><span class="keyword">;
<br>if (</span><span class="default">$zip</span><span class="keyword">-&gt;</span><span class="default">open</span><span class="keyword">(</span><span class="string">&apos;items.zip&apos;</span><span class="keyword">))
<br>{
<br>&#xA0; &#xA0;&#xA0; for(</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">$zip</span><span class="keyword">-&gt;</span><span class="default">numFiles</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++)
<br>&#xA0; &#xA0;&#xA0; {&#xA0;&#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;Filename: &apos; </span><span class="keyword">. </span><span class="default">$zip</span><span class="keyword">-&gt;</span><span class="default">getNameIndex</span><span class="keyword">(</span><span class="default">$i</span><span class="keyword">) . </span><span class="string">&apos;&lt;br /&gt;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0;&#xA0; }
<br>}
<br>else
<br>{
<br>&#xA0; &#xA0;&#xA0; echo </span><span class="string">&apos;Error reading zip-archive!&apos;</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Hope it helps.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/ziparchive.getnameindex.php)

**[⬆ to root](/)**