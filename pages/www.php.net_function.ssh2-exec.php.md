# ssh2_exec



There doesn&apos;t appear to be a way to get data from BOTH stderr and stdout streams simultaneously, or at least I&apos;ve yet to find one.<br><br>The following code *should* get the error message written to stderr, but if the call to stream_get_contents() for stdout is run first, the subsequent call for stderr won&apos;t return anything.  <br><br>If the order of the statements is reversed, the call for stderr will return any errors and call for stdout will return nothing<br><br>

```
<?php
// Run a command that will probably write to stderr (unless you have a folder named /hom)
$stream = ssh2_exec($connection, "cd /hom");
$errorStream = ssh2_fetch_stream($stream, SSH2_STREAM_STDERR);

// Enable blocking for both streams
stream_set_blocking($errorStream, true);
stream_set_blocking($stream, true);

// Whichever of the two below commands is listed first will receive its appropriate output.  The second command receives nothing
echo "Output: " . stream_get_contents($stream);
echo "Error: " . stream_get_contents($errorStream);

// Close the streams        
fclose($errorStream);
fclose($stream);

?>
```
<br><br>My initial suspicion is that either a) I&apos;ve done something wrong  or b) the blocking nature of both streams means that by the time first stream has received data and returned, the buffer for the second stream has already emptied.<br><br>I&apos;ve done preliminary tests with blocking disabled, but haven&apos;t had any luck there either.  

#

[Official documentation page](https://www.php.net/manual/en/function.ssh2-exec.php)

**[To root](/README.md)**