# unlink




<div class="phpcode"><span class="html">
This will delete all files in a directory matching a pattern in one line of code.
<br>
<br><span class="default">&lt;?php array_map</span><span class="keyword">(</span><span class="string">&apos;unlink&apos;</span><span class="keyword">, </span><span class="default">glob</span><span class="keyword">(</span><span class="string">&quot;some/dir/*.txt&quot;</span><span class="keyword">)); </span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Deleted a large file but seeing no increase in free space or decrease of disk usage? Using UNIX or other POSIX OS?<br><br>The unlink() is not about removing file, it&apos;s about removing a file name. The manpage says: ``unlink - delete a name and possibly the file it refers to&apos;&apos;.<br><br>Most of the time a file has just one name -- removing it will also remove (free, deallocate) the `body&apos; of file (with one caveat, see below). That&apos;s the simple, usual case.<br><br>However, it&apos;s perfectly fine for a file to have several names (see the link() function), in the same or different directories. All the names will refer to the file body and `keep it alive&apos;, so to say. Only when all the names are removed, the body of file actually is freed.<br><br>The caveat:<br>A file&apos;s body may *also* be `kept alive&apos; (still using diskspace) by a process holding the file open. The body will not be deallocated (will not free disk space) as long as the process holds it open. In fact, there&apos;s a fancy way of resurrecting a file removed by a mistake but still held open by a process...</span>
</div>
  

#


<div class="phpcode"><span class="html">
unlink($fileName); failed for me .<br>Then i tried using the realpath($fileName)&#xA0; function as <br>unlink(realpath($fileName)); it worked <br><br>just posting it , in case if any one finds it useful .</span>
</div>
  

#


<div class="phpcode"><span class="html">
I have been working on some little tryout where a backup file was created before modifying the main textfile. Then when an error is thrown, the main file will be deleted (unlinked) and the backup file is returned instead.<br><br>Though, I have been breaking my head for about an hour on why I couldn&apos;t get my persmissions right to unlink the main file.<br><br>Finally I knew what was wrong: because I was working on the file and hadn&apos;t yet closed the file, it was still in use and ofcourse couldn&apos;t be deleted :)<br><br>So I thought of mentoining this here, to avoid others of making the same mistake:<br><br><span class="default">&lt;?php<br></span><span class="comment">// First close the file<br></span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);<br><br></span><span class="comment">// Then unlink :)<br></span><span class="default">unlink</span><span class="keyword">(</span><span class="default">$somefile</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
To delete all files of a particular extension, or infact, delete all with wildcard, a much simplar way is to use the glob function.&#xA0; Say I wanted to delete all jpgs .........<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">foreach (</span><span class="default">glob</span><span class="keyword">(</span><span class="string">&quot;*.jpg&quot;</span><span class="keyword">) as </span><span class="default">$filename</span><span class="keyword">) {<br>&#xA0;&#xA0; echo </span><span class="string">&quot;</span><span class="default">$filename</span><span class="string"> size &quot; </span><span class="keyword">. </span><span class="default">filesize</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">) . </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>&#xA0;&#xA0; </span><span class="default">unlink</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">);<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here the simplest way to delete files with mask<br><br><span class="default">&lt;?php<br>&#xA0;&#xA0; $mask </span><span class="keyword">= </span><span class="string">&quot;*.jpg&quot;<br>&#xA0;&#xA0; </span><span class="default">array_map</span><span class="keyword">( </span><span class="string">&quot;unlink&quot;</span><span class="keyword">, </span><span class="default">glob</span><span class="keyword">( </span><span class="default">$mask </span><span class="keyword">) );<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
To anyone who&apos;s had a problem with the permissions denied error, it&apos;s sometimes caused when you try to delete a file that&apos;s in a folder higher in the hierarchy to your working directory (i.e. when trying to delete a path that starts with &quot;../&quot;).<br><br>So to work around this problem, you can use chdir() to change the working directory to the folder where the file you want to unlink is located.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; $old </span><span class="keyword">= </span><span class="default">getcwd</span><span class="keyword">(); </span><span class="comment">// Save the current directory<br>&#xA0; &#xA0; </span><span class="default">chdir</span><span class="keyword">(</span><span class="default">$path_to_file</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">unlink</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">chdir</span><span class="keyword">(</span><span class="default">$old</span><span class="keyword">); </span><span class="comment">// Restore the old working directory&#xA0; &#xA0; <br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.unlink.php)

**[⬆ to root](/)**