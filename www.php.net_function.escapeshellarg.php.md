# escapeshellarg




<div class="phpcode"><span class="html">
When escapeshellarg() was stripping my non-ASCII characters from a UTF-8 string, adding the following fixed the problem:<br><br><span class="default">&lt;?php<br>setlocale</span><span class="keyword">(</span><span class="default">LC_CTYPE</span><span class="keyword">, </span><span class="string">&quot;en_US.UTF-8&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Under Windows, this function puts string into double-quotes, not single, and replaces %(percent sign) with a space, that&apos;s why it&apos;s impossible to pass a filename with percents in its name through this function.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Most of the comments above have misunderstood this function. It does not need to escape characters such as &apos;$&apos; and &apos;`&apos; - it uses the fact that the shell does not treat any characters as special inside single quotes (except the single quote character itself). The correct way to use this function is to call it on a variable that is intended to be passed to a command-line program as a single argument to that program - you do not call it on command-line as a whole.<br><br>The person above who comments that this function behaves badly if given the empty string as input is correct - this is a bug. It should indeed return two single quotes in this case.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.escapeshellarg.php)

**[⬆ to root](/)**