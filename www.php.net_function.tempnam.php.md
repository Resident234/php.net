# tempnam




<div class="phpcode"><span class="html">
Watch out using a blank $dir as a &quot;trick&quot; to create temporary files in the system temporary directory.
<br>
<br><span class="default">&lt;?php
<br>$tmpfname </span><span class="keyword">= </span><span class="default">tempnam</span><span class="keyword">(</span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="string">&apos;FOO&apos;</span><span class="keyword">); </span><span class="comment">// not good
<br></span><span class="default">?&gt;
<br></span>
<br>If an open_basedir restriction is in effect, the trick will not work. You will get a warning message like
<br>
<br>Warning: tempnam() [function.tempnam]: open_basedir restriction in effect.
<br>File() is not within the allowed path(s): (/var/www/vhosts/example.com/httpdocs:/tmp)
<br>
<br>What works is this:
<br>
<br><span class="default">&lt;?php
<br>$tmpfname </span><span class="keyword">= </span><span class="default">tempnam</span><span class="keyword">(</span><span class="default">sys_get_temp_dir</span><span class="keyword">(), </span><span class="string">&apos;FOO&apos;</span><span class="keyword">); </span><span class="comment">// good
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Please note that this function might throw a notice in PHP 7.1.0 and above. This was a bugfix: <a href="https://bugs.php.net/bug.php?id=69489" rel="nofollow" target="_blank">https://bugs.php.net/bug.php?id=69489</a><br><br>You can place an address operator (@) to sillence the notice:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">if (</span><span class="default">$tmp </span><span class="keyword">= @</span><span class="default">tempnam</span><span class="keyword">() !== </span><span class="default">false</span><span class="keyword">) {<br>&#xA0; </span><span class="comment">// ...<br></span><span class="keyword">}<br><br></span><span class="default">?&gt;<br></span><br>Or you could try to set the &quot;upload_tmp_dir&quot; setting in your php.ini to the temporary folder path of your system. Not sure, if the last one prevents the notices.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that tempnam returns the full path to the temporary file, not just the filename.</span>
</div>
  

#


<div class="phpcode"><span class="html">
tempnam() function does not support custom stream wrappers registered by stream_register_wrapper(). <br><br>For example if you&apos;ll try to use tempnam() on Windows platform, PHP will try to generate unique filename in %TMP% folder (usually: C:\WINDOWS\Temp) without any warning or notice.<br><br><span class="default">&lt;?php<br><br></span><span class="comment">// &lt;&lt; ...custom stream wrapper goes somewhere here...&gt;&gt;<br><br></span><span class="keyword">echo </span><span class="string">&apos;&lt;pre&gt;&apos;</span><span class="keyword">;<br></span><span class="default">error_reporting</span><span class="keyword">(</span><span class="default">E_ALL</span><span class="keyword">);<br></span><span class="default">ini_set</span><span class="keyword">(</span><span class="string">&apos;display_errors&apos;</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br></span><span class="default">clearstatcache</span><span class="keyword">();<br></span><span class="default">stream_register_wrapper</span><span class="keyword">(</span><span class="string">&apos;test&apos;</span><span class="keyword">, </span><span class="string">&apos;MemoryStream&apos;</span><span class="keyword">);<br><br></span><span class="default">mkdir</span><span class="keyword">(</span><span class="string">&apos;test://aaa&apos;</span><span class="keyword">);<br></span><span class="default">mkdir</span><span class="keyword">(</span><span class="string">&apos;test://aaa/cc&apos;</span><span class="keyword">);<br></span><span class="default">mkdir</span><span class="keyword">(</span><span class="string">&apos;test://aaa/dd&apos;</span><span class="keyword">); <br>echo </span><span class="string">&apos;PHP &apos;</span><span class="keyword">.</span><span class="default">PHP_VERSION</span><span class="keyword">;<br>echo </span><span class="string">&apos;&lt;br /&gt;node exists: &apos;</span><span class="keyword">.</span><span class="default">file_exists</span><span class="keyword">(</span><span class="string">&apos;test://aaa/cc&apos;</span><span class="keyword">);<br>echo </span><span class="string">&apos;&lt;br /&gt;node is writable: &apos;</span><span class="keyword">.</span><span class="default">is_writable</span><span class="keyword">(</span><span class="string">&apos;test://aaa/cc&apos;</span><span class="keyword">);<br>echo </span><span class="string">&apos;&lt;br /&gt;node is dir: &apos;</span><span class="keyword">.</span><span class="default">is_dir</span><span class="keyword">(</span><span class="string">&apos;test://aaa/cc&apos;</span><span class="keyword">);<br>echo </span><span class="string">&apos;&lt;br /&gt;tempnam in dir: &apos;</span><span class="keyword">.</span><span class="default">tempnam</span><span class="keyword">(</span><span class="string">&apos;test://aaa/cc&apos;</span><span class="keyword">, </span><span class="string">&apos;tmp&apos;</span><span class="keyword">);<br>echo </span><span class="string">&quot;&lt;br /&gt;&lt;/pre&gt;&quot;</span><span class="keyword">;<br><br></span><span class="default">?&gt;<br></span><br>ouputs:<br>--------------------<br>PHP 5.2.13<br>node exists: 1<br>node is writable: 1<br>node is dir: 1<br>tempnam in dir: C:\Windows\Temp\tmp1D03.tmp<br><br>If you want to create temporary file, you have to create your own function (which will probably use opendir() and fopen($filename, &quot;x&quot;) functions)</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you go to the linux man page for the C function tempnam(3), you will see at the end &quot;Never use this function. Use mkstemp(3) instead.&quot; But php&apos;s tempnam() function doesn&apos;t actually use tmpnam(3), so there&apos;s no problem (under Linux, it will use mkstemp(3) if it&apos;s available).</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.tempnam.php)

**[To root](/README.md)**