# The SplFileObject class




<div class="phpcode"><span class="html">
Note that this class has a private (and thus, not documented) property that holds the file pointer. Combine this with the fact that there is no method to close the file handle, and you get into situations where you are not able to delete the file with unlink(), etc., because an SplFileObject still has a handle open.<br><br>To get around this issue, delete the SplFileObject like this:<br><br>---------------------------------------------------------------------<br><span class="default">&lt;?php<br></span><span class="keyword">print </span><span class="string">&quot;Declaring file object\n&quot;</span><span class="keyword">;<br></span><span class="default">$file </span><span class="keyword">= new </span><span class="default">SplFileObject</span><span class="keyword">(</span><span class="string">&apos;example.txt&apos;</span><span class="keyword">);<br><br>print </span><span class="string">&quot;Trying to delete file...\n&quot;</span><span class="keyword">;<br></span><span class="default">unlink</span><span class="keyword">(</span><span class="string">&apos;example.txt&apos;</span><span class="keyword">);<br><br>print </span><span class="string">&quot;Closing file object\n&quot;</span><span class="keyword">;<br></span><span class="default">$file </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;<br><br>print </span><span class="string">&quot;Deleting file...\n&quot;</span><span class="keyword">;<br></span><span class="default">unlink</span><span class="keyword">(</span><span class="string">&apos;example.txt&apos;</span><span class="keyword">);<br><br>print </span><span class="string">&apos;File deleted!&apos;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span>---------------------------------------------------------------------<br><br>which will output:<br><br>---------------------------------------------------------------------<br>Declaring file object <br>Trying to delete file... <br><br>Warning: unlink(example.txt): Permission denied in file.php on line 6<br>Closing file object <br>Deleting file... <br>File deleted!<br>---------------------------------------------------------------------</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.splfileobject.php)

**[⬆ to root](/)**