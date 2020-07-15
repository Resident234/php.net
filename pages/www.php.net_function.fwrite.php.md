# fwrite



After having problems with fwrite() returning 0 in cases where one would fully expect a return value of false, I took a look at the source code for php&apos;s fwrite() itself. The function will only return false if you pass in invalid arguments. Any other error, just as a broken pipe or closed connection, will result in a return value of less than strlen($string), in most cases 0.<br><br>Therefore, looping with repeated calls to fwrite() until the sum of number of bytes written equals the strlen() of the full value or expecting false on error will result in an infinite loop if the connection is lost.<br><br>This means the example fwrite_stream() code from the docs, as well as all the "helper" functions posted by others in the comments are all broken. You *must* check for a return value of 0 and either abort immediately or track a maximum number of retries.<br><br>Below is the example from the docs. This code is BAD, as a broken pipe will result in fwrite() infinitely looping with a return value of 0. Since the loop only breaks if fwrite() returns false or successfully writes all bytes, an infinite loop will occur on failure.<br><br>

```
<?php
// BROKEN function - infinite loop when fwrite() returns 0s
function fwrite_stream($fp, $string) {
    for ($written = 0; $written < strlen($string); $written += $fwrite) {
        $fwrite = fwrite($fp, substr($string, $written));
        if ($fwrite === false) {
            return $written;
        }
    }
    return $written;
}
?>
```
  

#

Some people say that when writing to a socket not all of the bytes requested to be written may be written. You may have to call fwrite again to write bytes that were not written the first time. (At least this is how the write() system call in UNIX works.)<br><br>This is helpful code (warning: not tested with multi-byte character sets)<br><br>function fwrite_with_retry($sock, &amp;$data)<br>{<br>    $bytes_to_write = strlen($data);<br>    $bytes_written = 0;<br><br>    while ( $bytes_written &lt; $bytes_to_write )<br>    {<br>        if ( $bytes_written == 0 ) {<br>            $rv = fwrite($sock, $data);<br>        } else {<br>            $rv = fwrite($sock, substr($data, $bytes_written));<br>        }<br><br>        if ( $rv === false || $rv == 0 )<br>            return( $bytes_written == 0 ? false : $bytes_written );<br><br>        $bytes_written += $rv;<br>    }<br><br>    return $bytes_written;<br>}<br><br>Call this like so:<br><br>    $rv = fwrite_with_retry($sock, $request_string);<br><br>    if ( ! $rv )<br>        die("unable to write request_string to socket");<br>    if ( $rv != strlen($request_string) )<br>        die("sort write to socket on writing request_string");  

#

[Official documentation page](https://www.php.net/manual/en/function.fwrite.php)

**[To root](/README.md)**