# inotify_init





Example for tailing a file (like tail -f) using inotify.


```
<?php
/**
 * Tail a file (UNIX only!)
 * Watch a file for changes using inotify and return the changed data
 *
 * @param string $file - filename of the file to be watched
 * @param integer $pos - actual position in the file
 * @return string
 */
function tail($file,&amp;$pos) {
&#xA0; &#xA0; // get the size of the file
&#xA0; &#xA0; if(!$pos) $pos = filesize($file);
&#xA0; &#xA0; // Open an inotify instance
&#xA0; &#xA0; $fd = inotify_init();
&#xA0; &#xA0; // Watch $file for changes.
&#xA0; &#xA0; $watch_descriptor = inotify_add_watch($fd, $file, IN_ALL_EVENTS);
&#xA0; &#xA0; // Loop forever (breaks are below)
&#xA0; &#xA0; while (true) {
&#xA0; &#xA0; &#xA0; &#xA0; // Read events (inotify_read is blocking!)
&#xA0; &#xA0; &#xA0; &#xA0; $events = inotify_read($fd);
&#xA0; &#xA0; &#xA0; &#xA0; // Loop though the events which occured
&#xA0; &#xA0; &#xA0; &#xA0; foreach ($events as $event=&gt;$evdetails) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // React on the event type
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; switch (true) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // File was modified
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case ($evdetails[&apos;mask&apos;] &amp; IN_MODIFY):
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Stop watching $file for changes
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; inotify_rm_watch($fd, $watch_descriptor);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Close the inotify instance
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fclose($fd);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // open the file
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $fp = fopen($file,&apos;r&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!$fp) return false;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // seek to the last EOF position
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fseek($fp,$pos);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // read until EOF
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; while (!feof($fp)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $buf .= fread($fp,8192);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // save the new EOF to $pos
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $pos = ftell($fp); // (remember: $pos is called by reference)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // close the file pointer
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fclose($fp);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // return the new data and leave the function
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $buf;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // be a nice guy and program good code ;-)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // File was moved or deleted
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case ($evdetails[&apos;mask&apos;] &amp; IN_MOVE):
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case ($evdetails[&apos;mask&apos;] &amp; IN_MOVE_SELF):
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case ($evdetails[&apos;mask&apos;] &amp; IN_DELETE):
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case ($evdetails[&apos;mask&apos;] &amp; IN_DELETE_SELF):
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Stop watching $file for changes
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; inotify_rm_watch($fd, $watch_descriptor);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Close the inotify instance
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fclose($fd);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Return a failure
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
}

// Use it like that:
$lastpos = 0;
while (true) {
&#xA0; &#xA0; echo tail($file,$lastpos);
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.inotify-init.php)

**[To root](/README.md)**