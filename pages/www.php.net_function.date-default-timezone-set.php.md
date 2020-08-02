# date_default_timezone_set



I&apos;m sure I&apos;m not the only one who is distressed by the recent default behavior change to E_NOTICE when the timezone isn&apos;t explicitly set in the program or in .ini.  I insure that the clock on the server IS correct, and I don&apos;t want to have to set it in two places (the system AND PHP).  So I want to read it from the system.  But PHP won&apos;t accept that answer, and insists on a call to this function.  So, here&apos;s my answer:<br><br>

```
<?php
 function setTimezone($default) {
    $timezone = "";
    
    // On many systems (Mac, for instance) "/etc/localtime" is a symlink
    // to the file with the timezone info
    if (is_link("/etc/localtime")) {
        
        // If it is, that file's name is actually the "Olsen" format timezone
        $filename = readlink("/etc/localtime");
        
        $pos = strpos($filename, "zoneinfo");
        if ($pos) {
            // When it is, it's in the "/usr/share/zoneinfo/" folder
            $timezone = substr($filename, $pos + strlen("zoneinfo/"));
        } else {
            // If not, bail
            $timezone = $default;
        }
    }
    else {
        // On other systems, like Ubuntu, there's file with the Olsen time
        // right inside it.
        $timezone = file_get_contents("/etc/timezone");
        if (!strlen($timezone)) {
            $timezone = $default;
        }
    }
    date_default_timezone_set($timezone);
 }
?>
```
<br><br>Use it by calling it with a fallback default answer.<br><br>Yes, I know it doesn&apos;t work on Windows.  Neither do I :)  Perhaps someone wants to add that functionality.<br><br>Hope this helps someone.  

---

[Official documentation page](https://www.php.net/manual/en/function.date-default-timezone-set.php)

**[To root](/README.md)**