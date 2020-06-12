# fwrite




<div class="phpcode"><span class="html">
After having problems with fwrite() returning 0 in cases where one would fully expect a return value of false, I took a look at the source code for php&apos;s fwrite() itself. The function will only return false if you pass in invalid arguments. Any other error, just as a broken pipe or closed connection, will result in a return value of less than strlen($string), in most cases 0.
<br>
<br>Therefore, looping with repeated calls to fwrite() until the sum of number of bytes written equals the strlen() of the full value or expecting false on error will result in an infinite loop if the connection is lost.
<br>
<br>This means the example fwrite_stream() code from the docs, as well as all the &quot;helper&quot; functions posted by others in the comments are all broken. You *must* check for a return value of 0 and either abort immediately or track a maximum number of retries.
<br>
<br>Below is the example from the docs. This code is BAD, as a broken pipe will result in fwrite() infinitely looping with a return value of 0. Since the loop only breaks if fwrite() returns false or successfully writes all bytes, an infinite loop will occur on failure.
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">// BROKEN function - infinite loop when fwrite() returns 0s
<br></span><span class="keyword">function </span><span class="default">fwrite_stream</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">$string</span><span class="keyword">) {
<br>&#xA0; &#xA0; for (</span><span class="default">$written </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$written </span><span class="keyword">&lt; </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">); </span><span class="default">$written </span><span class="keyword">+= </span><span class="default">$fwrite</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$fwrite </span><span class="keyword">= </span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">, </span><span class="default">$written</span><span class="keyword">));
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$fwrite </span><span class="keyword">=== </span><span class="default">false</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$written</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return </span><span class="default">$written</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Some people say that when writing to a socket not all of the bytes requested to be written may be written. You may have to call fwrite again to write bytes that were not written the first time. (At least this is how the write() system call in UNIX works.)<br><br>This is helpful code (warning: not tested with multi-byte character sets)<br><br>function fwrite_with_retry($sock, &amp;$data)<br>{<br>&#xA0; &#xA0; $bytes_to_write = strlen($data);<br>&#xA0; &#xA0; $bytes_written = 0;<br><br>&#xA0; &#xA0; while ( $bytes_written &lt; $bytes_to_write )<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; if ( $bytes_written == 0 ) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $rv = fwrite($sock, $data);<br>&#xA0; &#xA0; &#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $rv = fwrite($sock, substr($data, $bytes_written));<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br><br>&#xA0; &#xA0; &#xA0; &#xA0; if ( $rv === false || $rv == 0 )<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return( $bytes_written == 0 ? false : $bytes_written );<br><br>&#xA0; &#xA0; &#xA0; &#xA0; $bytes_written += $rv;<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; return $bytes_written;<br>}<br><br>Call this like so:<br><br>&#xA0; &#xA0; $rv = fwrite_with_retry($sock, $request_string);<br><br>&#xA0; &#xA0; if ( ! $rv )<br>&#xA0; &#xA0; &#xA0; &#xA0; die(&quot;unable to write request_string to socket&quot;);<br>&#xA0; &#xA0; if ( $rv != strlen($request_string) )<br>&#xA0; &#xA0; &#xA0; &#xA0; die(&quot;sort write to socket on writing request_string&quot;);</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.fwrite.php)

**[⬆ to root](/)**