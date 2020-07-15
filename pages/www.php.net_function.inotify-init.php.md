# inotify_init



Example for tailing a file (like tail -f) using inotify.<br>

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
    // get the size of the file
    if(!$pos) $pos = filesize($file);
    // Open an inotify instance
    $fd = inotify_init();
    // Watch $file for changes.
    $watch_descriptor = inotify_add_watch($fd, $file, IN_ALL_EVENTS);
    // Loop forever (breaks are below)
    while (true) {
        // Read events (inotify_read is blocking!)
        $events = inotify_read($fd);
        // Loop though the events which occured
        foreach ($events as $event=>$evdetails) {
            // React on the event type
            switch (true) {
                // File was modified
                case ($evdetails['mask'] &amp; IN_MODIFY):
                    // Stop watching $file for changes
                    inotify_rm_watch($fd, $watch_descriptor);
                    // Close the inotify instance
                    fclose($fd);
                    // open the file
                    $fp = fopen($file,'r');
                    if (!$fp) return false;
                    // seek to the last EOF position
                    fseek($fp,$pos);
                    // read until EOF
                    while (!feof($fp)) {
                        $buf .= fread($fp,8192);
                    }
                    // save the new EOF to $pos
                    $pos = ftell($fp); // (remember: $pos is called by reference)
                    // close the file pointer
                    fclose($fp);
                    // return the new data and leave the function
                    return $buf;
                    // be a nice guy and program good code ;-)
                    break;

                    // File was moved or deleted
                case ($evdetails['mask'] &amp; IN_MOVE):
                case ($evdetails['mask'] &amp; IN_MOVE_SELF):
                case ($evdetails['mask'] &amp; IN_DELETE):
                case ($evdetails['mask'] &amp; IN_DELETE_SELF):
                    // Stop watching $file for changes
                    inotify_rm_watch($fd, $watch_descriptor);
                    // Close the inotify instance
                    fclose($fd);
                    // Return a failure
                    return false;
                    break;
            }
        }
    }
}

// Use it like that:
$lastpos = 0;
while (true) {
    echo tail($file,$lastpos);
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.inotify-init.php)

**[To root](/README.md)**