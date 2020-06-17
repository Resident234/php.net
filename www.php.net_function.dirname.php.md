# dirname




<div class="phpcode"><span class="html">
As of PHP 5.3.0, you can use __DIR__ as a replacement for dirname(__FILE__)</span>
</div>
  

#


<div class="phpcode"><span class="html">
To get the directory of current included file:
<br>
<br><span class="default">&lt;?php
<br>dirname</span><span class="keyword">(</span><span class="default">__FILE__</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>For example, if a script called &apos;database.init.php&apos; which is included from anywhere on the filesystem wants to include the script &apos;database.class.php&apos;, which lays in the same directory, you can use:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">include_once(</span><span class="default">dirname</span><span class="keyword">(</span><span class="default">__FILE__</span><span class="keyword">) . </span><span class="string">&apos;/database.class.php&apos;</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Since the paths in the examples given only have two parts (e.g. &quot;/etc/passwd&quot;) it is not obvious whether dirname returns the single path element of the parent directory or whether it returns the whole path up to and including the parent directory.&#xA0; From experimentation it appears to be the latter.<br><br>e.g. <br><br>dirname(&apos;/usr/local/magic/bin&apos;);<br><br>returns &apos;/usr/local/magic&apos;&#xA0; and not just &apos;magic&apos;<br><br>Also it is not immediately obvious that dirname effectively returns the parent directory of the last item of the path regardless of whether the last item is a directory or a file.&#xA0; (i.e. one might think that if the path given was a directory then dirname would return the entire original path since that is a directory name.)<br><br>Further the presense of a directory separator at the end of the path does not necessarily indicate that last item of the path is a directory, and so <br><br>dirname(&apos;/usr/local/magic/bin/&apos;);&#xA0; #note final &apos;/&apos;<br><br>would return the same result as in my example above.<br><br>In short this seems to be more of a string manipulation function that strips off the last non-null file or directory element off of a path string.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.dirname.php)

**[To root](/README.md)**