# date_default_timezone_set





I&apos;m sure I&apos;m not the only one who is distressed by the recent default behavior change to E_NOTICE when the timezone isn&apos;t explicitly set in the program or in .ini.&#xA0; I insure that the clock on the server IS correct, and I don&apos;t want to have to set it in two places (the system AND PHP).&#xA0; So I want to read it from the system.&#xA0; But PHP won&apos;t accept that answer, and insists on a call to this function.&#xA0; So, here&apos;s my answer:



```
<?php
 function setTimezone($default) {
&#xA0; &#xA0; $timezone = &quot;&quot;;
&#xA0; &#xA0; 
&#xA0; &#xA0; // On many systems (Mac, for instance) &quot;/etc/localtime&quot; is a symlink
&#xA0; &#xA0; // to the file with the timezone info
&#xA0; &#xA0; if (is_link(&quot;/etc/localtime&quot;)) {
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; // If it is, that file&apos;s name is actually the &quot;Olsen&quot; format timezone
&#xA0; &#xA0; &#xA0; &#xA0; $filename = readlink(&quot;/etc/localtime&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; $pos = strpos($filename, &quot;zoneinfo&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; if ($pos) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // When it is, it&apos;s in the &quot;/usr/share/zoneinfo/&quot; folder
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $timezone = substr($filename, $pos + strlen(&quot;zoneinfo/&quot;));
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // If not, bail
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $timezone = $default;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; else {
&#xA0; &#xA0; &#xA0; &#xA0; // On other systems, like Ubuntu, there&apos;s file with the Olsen time
&#xA0; &#xA0; &#xA0; &#xA0; // right inside it.
&#xA0; &#xA0; &#xA0; &#xA0; $timezone = file_get_contents(&quot;/etc/timezone&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; if (!strlen($timezone)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $timezone = $default;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; date_default_timezone_set($timezone);
 }
?>
```


Use it by calling it with a fallback default answer.

Yes, I know it doesn&apos;t work on Windows.&#xA0; Neither do I :)&#xA0; Perhaps someone wants to add that functionality.

Hope this helps someone.

  

#

[Official documentation page](https://www.php.net/manual/en/function.date-default-timezone-set.php)

**[To root](/README.md)**