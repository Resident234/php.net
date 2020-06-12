# include_once




<div class="phpcode"><span class="html">
In response to what a user wrote 8 years ago regarding include_once being ran two times in a row on a non-existent file:<br><br>Perhaps 8 years ago that was the case, however I have tested in PHP 5.6, and I get this:<br><br>$result = include_once &apos;fakefile.php&apos;;&#xA0; // $result = false<br>$result = include_once &apos;fakefile.php&apos;&#xA0;&#xA0; // $result is still false</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you include a file that does not exist with include_once, the return result will be false. <br><br>If you try to include that same file again with include_once the return value will be true.<br><br>Example:<br><span class="default">&lt;?php<br>var_dump</span><span class="keyword">(include_once </span><span class="string">&apos;fakefile.ext&apos;</span><span class="keyword">); </span><span class="comment">// bool(false)<br></span><span class="default">var_dump</span><span class="keyword">(include_once </span><span class="string">&apos;fakefile.ext&apos;</span><span class="keyword">); </span><span class="comment">// bool(true)<br></span><span class="default">?&gt;<br></span><br>This is because according to php the file was already included once (even though it does not exist).</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.include-once.php)

**[To root](/README.md)**