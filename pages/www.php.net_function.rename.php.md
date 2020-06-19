# rename




<div class="phpcode"><span class="html">
Code first, then explanation.<br><br><span class="default">&lt;?php<br><br> rename </span><span class="keyword">(</span><span class="string">&quot;/folder/file.ext&quot;</span><span class="keyword">, </span><span class="string">&quot;newfile.ext&quot;</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>That doesn&apos;t rename the file within the folder, as you might assume, instead, it moves the file to whatever the PHP working directory is... Chances are you&apos;ll not find it in your FTP tree. Instead, use the following:<br><br><span class="default">&lt;?php<br><br> rename </span><span class="keyword">(</span><span class="string">&quot;/folder/file.ext&quot;</span><span class="keyword">, </span><span class="string">&quot;/folder/newfile.ext&quot;</span><span class="keyword">);<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
From the Changelog notes:<br><br>&quot;Warnings may be generated if the destination filesystem doesn&apos;t permit chown() or chmod() system calls to be made on files &#x2014; for example, if the destination filesystem is a FAT filesystem.&quot;<br><br>More explicitly, rename() may still return (bool) true, despite the warnings that result from the underlying calls to chown() or chmod(). This behavior can be misleading absent a deeper understanding of the underlying mechanics. To rename across filesystems, PHP &quot;fakes it&quot; by calling copy(), unlink(), chown(), and chmod() (not necessarily in that order). See PHP bug #50676 for more information.<br><br>On UNIX-like operating systems, filesystems may be mounted with an explicit uid and/or gid (for example, with mount options &quot;uid=someuser,gid=somegroup&quot;). Attempting to call rename() with such a destination filesystem will cause an &quot;Operation not permitted&quot; warning, even though the file is indeed renamed and rename() returns (bool) true.<br><br>This is not a bug. Either handle the warning as is appropriate to your use-case, or call copy() and then unlink(), which will avoid the doomed calls to chown() and chmod(), thereby eliminating the warning.</span>
</div>
  

#


<div class="phpcode"><span class="html">
For those who are still confused about the behavior of rename() in Linux and Windows (Windows XP) when target destination exists:<br><br>I have tested rename($oldName, $targetName) in PHP 5.3.0 and PHP 5.2.9 on both OS and find that if a file named $targetName does exist before, it will be overwritten with the content of $oldName</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note, that on Unix, a rename is a beautiful way of getting atomic updates to files.
<br>
<br>Just copy the old contents (if necessary), and write the new contents into a new file, then rename over the original file.
<br>
<br>Any processes reading from the file will continue to do so, any processes trying to open the file while you&apos;re writing to it will get the old file (because you&apos;ll be writing to a temp file), and there is no &quot;intermediate&quot; time between there being a file, and there not being a file (or there being half a file).
<br>
<br>Oh, and this only works if you have the temp file and the destination file on the same filesystem (eg. partition/hard-disk).</span>
</div>
  

#


<div class="phpcode"><span class="html">
If by any chance you end up with something equivalent to this:<br><br><span class="default">&lt;?php<br>rename</span><span class="keyword">(</span><span class="string">&apos;/foo/bar&apos;</span><span class="keyword">,</span><span class="string">&apos;/foo/bar&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>It returns true. It&apos;s not documented.</span>
</div>
  

#


<div class="phpcode"><span class="html">
rename() is working on Linux/UNIX but not working on Windows on a directory containing a file formerly opened within the same script. The problem persists even after properly closing the file and flushing the buffer.<br><br><span class="default">&lt;?php<br>$fp </span><span class="keyword">= </span><span class="default">fopen </span><span class="keyword">(</span><span class="string">&quot;./dir/ex.txt&quot; </span><span class="keyword">, </span><span class="string">&quot;r+&quot;</span><span class="keyword">);<br></span><span class="default">$text </span><span class="keyword">= </span><span class="default">fread</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">filesize</span><span class="keyword">(</span><span class="string">&quot;../dir/ex.txt&quot;</span><span class="keyword">));<br></span><span class="default">ftruncate</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">);<br></span><span class="default">$text2 </span><span class="keyword">= </span><span class="string">&quot;some value&quot;</span><span class="keyword">;<br></span><span class="default">fwrite </span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">,&#xA0; </span><span class="default">$text2 </span><span class="keyword">);<br></span><span class="default">fflush</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);<br></span><span class="default">fclose </span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);<br>if (</span><span class="default">is_resource</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">))<br>&#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);<br></span><span class="default">rename </span><span class="keyword">(</span><span class="string">&quot;./dir&quot;</span><span class="keyword">, </span><span class="string">&quot;.dir2&quot;</span><span class="keyword">); </span><span class="comment">// will give a &#xAB;permission denied&#xBB; error<br></span><span class="default">?&gt;<br></span><br>Strangely it seem that the rename command is&#xA0; executed BEFORE the handle closing on Windows.<br><br>Inserting a sleep() command before the renaming cures the problem.<br><br><span class="default">&lt;?php<br>$fp </span><span class="keyword">= </span><span class="default">fopen </span><span class="keyword">(</span><span class="string">&quot;./dir/ex.txt&quot; </span><span class="keyword">, </span><span class="string">&quot;r+&quot;</span><span class="keyword">);<br></span><span class="default">$text </span><span class="keyword">= </span><span class="default">fread</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">filesize</span><span class="keyword">(</span><span class="string">&quot;../dir/ex.txt&quot;</span><span class="keyword">));<br></span><span class="default">ftruncate</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">);<br></span><span class="default">$text2 </span><span class="keyword">= </span><span class="string">&quot;some value&quot;</span><span class="keyword">;<br></span><span class="default">fwrite </span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">,&#xA0; </span><span class="default">$text2 </span><span class="keyword">);<br></span><span class="default">fflush</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);<br></span><span class="default">fclose </span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);<br>if (</span><span class="default">is_resource</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">))<br>&#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);<br></span><span class="default">sleep</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">);&#xA0; &#xA0; </span><span class="comment">// this does the trick<br></span><span class="default">rename </span><span class="keyword">(</span><span class="string">&quot;./dir&quot;</span><span class="keyword">, </span><span class="string">&quot;.dir2&quot;</span><span class="keyword">); </span><span class="comment">//no error <br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.rename.php)

**[To root](/README.md)**