# timezone_offset_get



Hi,<br>This might help someone with timezone differences. I wanted to find the offset in seconds between my timezone and a remote timezone so I wrote this function and it seems to work well for me.<br><br>

```
<?php
/**    Returns the offset from the origin timezone to the remote timezone, in seconds.
*    @param $remote_tz;
*    @param $origin_tz; If null the servers current timezone is used as the origin.
*    @return int;
*/
function get_timezone_offset($remote_tz, $origin_tz = null) {
    if($origin_tz === null) {
        if(!is_string($origin_tz = date_default_timezone_get())) {
            return false; // A UTC timestamp was returned -- bail out!
        }
    }
    $origin_dtz = new DateTimeZone($origin_tz);
    $remote_dtz = new DateTimeZone($remote_tz);
    $origin_dt = new DateTime("now", $origin_dtz);
    $remote_dt = new DateTime("now", $remote_dtz);
    $offset = $origin_dtz-&gt;getOffset($origin_dt) - $remote_dtz-&gt;getOffset($remote_dt);
    return $offset;
}
?>
```

Examples:


```
<?php
// This will return 10800 (3 hours) ...
$offset = get_timezone_offset(&apos;America/Los_Angeles&apos;,&apos;America/New_York&apos;);
// or, if your server time is already set to &apos;America/New_York&apos;...
$offset = get_timezone_offset(&apos;America/Los_Angeles&apos;);
// You can then take $offset and adjust your timestamp.
$offset_time = time() + $offset;
?>
```
<br><br>Happy Coding!!  

#

[Official documentation page](https://www.php.net/manual/en/function.timezone-offset-get.php)

**[To root](/README.md)**