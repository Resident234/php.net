# fwrite





After having problems with fwrite() returning 0 in cases where one would fully expect a return value of false, I took a look at the source code for php&apos;s fwrite() itself. The function will only return false if you pass in invalid arguments. Any other error, just as a broken pipe or closed connection, will result in a return value of less than strlen($string), in most cases 0.



Therefore, looping with repeated calls to fwrite() until the sum of number of bytes written equals the strlen() of the full value or expecting false on error will result in an infinite loop if the connection is lost.



This means the example fwrite_stream() code from the docs, as well as all the &quot;helper&quot; functions posted by others in the comments are all broken. You *must* check for a return value of 0 and either abort immediately or track a maximum number of retries.



Below is the example from the docs. This code is BAD, as a broken pipe will result in fwrite() infinitely looping with a return value of 0. Since the loop only breaks if fwrite() returns false or successfully writes all bytes, an infinite loop will occur on failure.





```
<?php

// BROKEN function - infinite loop when fwrite() returns 0s

function fwrite_stream($fp, $string) {

&#xA0; &#xA0; for ($written = 0; $written &lt; strlen($string); $written += $fwrite) {

&#xA0; &#xA0; &#xA0; &#xA0; $fwrite = fwrite($fp, substr($string, $written));

&#xA0; &#xA0; &#xA0; &#xA0; if ($fwrite === false) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $written;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

&#xA0; &#xA0; return $written;

}

?>
```



  

#



Some people say that when writing to a socket not all of the bytes requested to be written may be written. You may have to call fwrite again to write bytes that were not written the first time. (At least this is how the write() system call in UNIX works.)

This is helpful code (warning: not tested with multi-byte character sets)

function fwrite_with_retry($sock, &amp;$data)
{
&#xA0; &#xA0; $bytes_to_write = strlen($data);
&#xA0; &#xA0; $bytes_written = 0;

&#xA0; &#xA0; while ( $bytes_written &lt; $bytes_to_write )
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if ( $bytes_written == 0 ) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $rv = fwrite($sock, $data);
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $rv = fwrite($sock, substr($data, $bytes_written));
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; if ( $rv === false || $rv == 0 )
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return( $bytes_written == 0 ? false : $bytes_written );

&#xA0; &#xA0; &#xA0; &#xA0; $bytes_written += $rv;
&#xA0; &#xA0; }

&#xA0; &#xA0; return $bytes_written;
}

Call this like so:

&#xA0; &#xA0; $rv = fwrite_with_retry($sock, $request_string);

&#xA0; &#xA0; if ( ! $rv )
&#xA0; &#xA0; &#xA0; &#xA0; die(&quot;unable to write request_string to socket&quot;);
&#xA0; &#xA0; if ( $rv != strlen($request_string) )
&#xA0; &#xA0; &#xA0; &#xA0; die(&quot;sort write to socket on writing request_string&quot;);

  

#

[Official documentation page](https://www.php.net/manual/en/function.fwrite.php)

**[To root](/README.md)**