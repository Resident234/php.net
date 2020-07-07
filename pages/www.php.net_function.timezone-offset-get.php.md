# timezone_offset_get





Hi,
This might help someone with timezone differences. I wanted to find the offset in seconds between my timezone and a remote timezone so I wrote this function and it seems to work well for me.



```
<?php
/**&#xA0; &#xA0; Returns the offset from the origin timezone to the remote timezone, in seconds.
*&#xA0; &#xA0; @param $remote_tz;
*&#xA0; &#xA0; @param $origin_tz; If null the servers current timezone is used as the origin.
*&#xA0; &#xA0; @return int;
*/
function get_timezone_offset($remote_tz, $origin_tz = null) {
&#xA0; &#xA0; if($origin_tz === null) {
&#xA0; &#xA0; &#xA0; &#xA0; if(!is_string($origin_tz = date_default_timezone_get())) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false; // A UTC timestamp was returned -- bail out!
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; $origin_dtz = new DateTimeZone($origin_tz);
&#xA0; &#xA0; $remote_dtz = new DateTimeZone($remote_tz);
&#xA0; &#xA0; $origin_dt = new DateTime(&quot;now&quot;, $origin_dtz);
&#xA0; &#xA0; $remote_dt = new DateTime(&quot;now&quot;, $remote_dtz);
&#xA0; &#xA0; $offset = $origin_dtz-&gt;getOffset($origin_dt) - $remote_dtz-&gt;getOffset($remote_dt);
&#xA0; &#xA0; return $offset;
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


Happy Coding!!

  

#

[Official documentation page](https://www.php.net/manual/en/function.timezone-offset-get.php)

**[To root](/README.md)**