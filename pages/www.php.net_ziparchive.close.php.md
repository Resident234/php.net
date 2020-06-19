# ZipArchive::close




<div class="phpcode"><span class="html">
It may seem a little obvious to some but it was an oversight on my behalf.<br><br>If you are adding files to the zip file that you want to be deleted make sure you delete AFTER you call the close() function.<br><br>If the files added to the object aren&apos;t available at save time the zip file will not be created.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you&apos;re adding multiple files to a zip and your $zip-&gt;close() call is returning FALSE, ensure that all the files you added actually exist. Apparently $zip-&gt;addFile() returns TRUE even if the file doesn&apos;t actually exist. It&apos;s a good idea to check each file with file_exists() or is_readable() before calling $zip-&gt;addFile() on it.</span>
</div>
  

#


<div class="phpcode"><span class="html">
ZipArchive.close() changes its behaviour in PHP7. The function ignores directories in PHP5 but fails in PHP7 with: <br><br>Unexpected PHP error [ZipArchive::close(): Read error: Is a directory]<br><br>The following code works in PHP5 but not in PHP7:<br><br><span class="default">&lt;?php <br></span><span class="comment">// test.php<br></span><span class="default">$zip </span><span class="keyword">= new </span><span class="default">ZipArchive</span><span class="keyword">();<br></span><span class="default">$zip</span><span class="keyword">-&gt;</span><span class="default">open</span><span class="keyword">(</span><span class="string">&apos;/tmp/test.zip&apos;</span><span class="keyword">, </span><span class="default">ZipArchive</span><span class="keyword">::</span><span class="default">CREATE</span><span class="keyword">);<br></span><span class="default">$zip</span><span class="keyword">-&gt;</span><span class="default">addFile</span><span class="keyword">(</span><span class="string">&apos;.&apos;</span><span class="keyword">, </span><span class="string">&apos;.&apos;</span><span class="keyword">);<br></span><span class="default">$ret </span><span class="keyword">= </span><span class="default">$zip</span><span class="keyword">-&gt;</span><span class="default">close</span><span class="keyword">();<br>echo </span><span class="string">&quot;Closed with: &quot; </span><span class="keyword">. (</span><span class="default">$ret </span><span class="keyword">? </span><span class="string">&quot;true&quot; </span><span class="keyword">: </span><span class="string">&quot;false&quot;</span><span class="keyword">) . </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>For php5:<br><br>php --version<br>&#xA0; PHP 5.5.38-1-avature-ondrej-fork (cli) (built: Aug 31 2016 16:37:38)<br><br>php test.php<br>&#xA0; Closed with: true<br><br>For php7:<br><br>php --version<br>&#xA0; PHP 7.0.8-0ubuntu0.16.04.2 (cli) ( NTS )<br><br>php test.php<br>&#xA0; Closed with: false</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/ziparchive.close.php)

**[To root](/README.md)**