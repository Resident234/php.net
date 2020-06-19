# symlink




<div class="phpcode"><span class="html">
Here is a simple way to control who downloads your files...
<br>
<br>You will have to set: $filename, $downloaddir, $safedir and $downloadURL.
<br>
<br>Basically $filename is the name of a file, $downloaddir is any dir on your server, $safedir is a dir that is not accessible by a browser that contains a file named $filename and $downloadURL is the URL equivalent of your $downloaddir.
<br>
<br>The way this works is when a user wants to download a file, a randomly named dir is created in the $downloaddir, and a symbolic link is created to the file being requested.&#xA0; The browser is then redirected to the new link and the download begins.
<br>
<br>The code also deletes any past symbolic links created by any past users before creating one for itself.&#xA0; This in effect leaves only one symbolic link at a time and prevents past users from downloading the file again without going through this script.&#xA0; There appears to be no problem if a symbolic link is deleted while another person is downloading from that link.
<br>
<br>This is not too great if not many people download the file since the symbolic link will not be deleted until another person downloads the same file. 
<br>
<br>Anyway enjoy:
<br>
<br><span class="default">&lt;?php
<br>$letters </span><span class="keyword">= </span><span class="string">&apos;abcdefghijklmnopqrstuvwxyz&apos;</span><span class="keyword">;
<br></span><span class="default">srand</span><span class="keyword">((double) </span><span class="default">microtime</span><span class="keyword">() * </span><span class="default">1000000</span><span class="keyword">);
<br></span><span class="default">$string </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt;= </span><span class="default">rand</span><span class="keyword">(</span><span class="default">4</span><span class="keyword">,</span><span class="default">12</span><span class="keyword">); </span><span class="default">$i</span><span class="keyword">++) {
<br>&#xA0;&#xA0; </span><span class="default">$q </span><span class="keyword">= </span><span class="default">rand</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">,</span><span class="default">24</span><span class="keyword">);
<br>&#xA0;&#xA0; </span><span class="default">$string </span><span class="keyword">= </span><span class="default">$string </span><span class="keyword">. </span><span class="default">$letters</span><span class="keyword">[</span><span class="default">$q</span><span class="keyword">];
<br>}
<br></span><span class="default">$handle </span><span class="keyword">= </span><span class="default">opendir</span><span class="keyword">(</span><span class="default">$downloaddir</span><span class="keyword">);
<br>while (</span><span class="default">$dir </span><span class="keyword">= </span><span class="default">readdir</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">)) {
<br>&#xA0;&#xA0; if (</span><span class="default">is_dir</span><span class="keyword">(</span><span class="default">$downloaddir </span><span class="keyword">. </span><span class="default">$dir</span><span class="keyword">)){
<br>&#xA0; &#xA0; &#xA0; if (</span><span class="default">$dir </span><span class="keyword">!= </span><span class="string">&quot;.&quot; </span><span class="keyword">&amp;&amp; </span><span class="default">$dir </span><span class="keyword">!= </span><span class="string">&quot;..&quot;</span><span class="keyword">){
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; @</span><span class="default">unlink</span><span class="keyword">(</span><span class="default">$downloaddir </span><span class="keyword">. </span><span class="default">$dir </span><span class="keyword">. </span><span class="string">&quot;/&quot; </span><span class="keyword">. </span><span class="default">$filename</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; @</span><span class="default">rmdir</span><span class="keyword">(</span><span class="default">$downloaddir </span><span class="keyword">. </span><span class="default">$dir</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; }
<br>&#xA0;&#xA0; }
<br>}
<br></span><span class="default">closedir</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">);
<br></span><span class="default">mkdir</span><span class="keyword">(</span><span class="default">$downloaddir </span><span class="keyword">. </span><span class="default">$string</span><span class="keyword">, </span><span class="default">0777</span><span class="keyword">);
<br></span><span class="default">symlink</span><span class="keyword">(</span><span class="default">$safedir </span><span class="keyword">. </span><span class="default">$filename</span><span class="keyword">, </span><span class="default">$downloaddir </span><span class="keyword">. </span><span class="default">$string </span><span class="keyword">. </span><span class="string">&quot;/&quot; </span><span class="keyword">. </span><span class="default">$filename</span><span class="keyword">);
<br></span><span class="default">Header</span><span class="keyword">(</span><span class="string">&quot;Location: &quot; </span><span class="keyword">. </span><span class="default">$downloadURL </span><span class="keyword">. </span><span class="default">$string </span><span class="keyword">. </span><span class="string">&quot;/&quot; </span><span class="keyword">. </span><span class="default">$filename</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Symlinks on windows&#xA0; are created by Symlink() which accept only absolute paths&#xA0; but not relative paths .relative paths on windows are not supported for symlinks</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.symlink.php)

**[To root](/README.md)**