# date_parse_from_format





For use in Versions prior V5.3:



```
<?php
if (!function_exists(&apos;date_parse_from_format&apos;)) {

&#xA0; &#xA0; function date_parse_from_format($format, $date) {
&#xA0; &#xA0; &#xA0; &#xA0; // reverse engineer date formats
&#xA0; &#xA0; &#xA0; &#xA0; $keys = array(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;Y&apos; =&gt; array(&apos;year&apos;, &apos;\d{4}&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;y&apos; =&gt; array(&apos;year&apos;, &apos;\d{2}&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;m&apos; =&gt; array(&apos;month&apos;, &apos;\d{2}&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;n&apos; =&gt; array(&apos;month&apos;, &apos;\d{1,2}&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;M&apos; =&gt; array(&apos;month&apos;, &apos;[A-Z][a-z]{3}&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;F&apos; =&gt; array(&apos;month&apos;, &apos;[A-Z][a-z]{2,8}&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;d&apos; =&gt; array(&apos;day&apos;, &apos;\d{2}&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;j&apos; =&gt; array(&apos;day&apos;, &apos;\d{1,2}&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;D&apos; =&gt; array(&apos;day&apos;, &apos;[A-Z][a-z]{2}&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;l&apos; =&gt; array(&apos;day&apos;, &apos;[A-Z][a-z]{6,9}&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;u&apos; =&gt; array(&apos;hour&apos;, &apos;\d{1,6}&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;h&apos; =&gt; array(&apos;hour&apos;, &apos;\d{2}&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;H&apos; =&gt; array(&apos;hour&apos;, &apos;\d{2}&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;g&apos; =&gt; array(&apos;hour&apos;, &apos;\d{1,2}&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;G&apos; =&gt; array(&apos;hour&apos;, &apos;\d{1,2}&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;i&apos; =&gt; array(&apos;minute&apos;, &apos;\d{2}&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;s&apos; =&gt; array(&apos;second&apos;, &apos;\d{2}&apos;)
&#xA0; &#xA0; &#xA0; &#xA0; );

&#xA0; &#xA0; &#xA0; &#xA0; // convert format string to regex
&#xA0; &#xA0; &#xA0; &#xA0; $regex = &apos;&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; $chars = str_split($format);
&#xA0; &#xA0; &#xA0; &#xA0; foreach ($chars AS $n =&gt; $char) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $lastChar = isset($chars[$n - 1]) ? $chars[$n - 1] : &apos;&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $skipCurrent = &apos;\\&apos; == $lastChar;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!$skipCurrent &amp;&amp; isset($keys[$char])) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $regex .= &apos;(?P&lt;&apos; . $keys[$char][0] . &apos;&gt;&apos; . $keys[$char][1] . &apos;)&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else if (&apos;\\&apos; == $char) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $regex .= $char;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $regex .= preg_quote($char);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; $dt = array();
&#xA0; &#xA0; &#xA0; &#xA0; $dt[&apos;error_count&apos;] = 0;
&#xA0; &#xA0; &#xA0; &#xA0; // now try to match it
&#xA0; &#xA0; &#xA0; &#xA0; if (preg_match(&apos;#^&apos; . $regex . &apos;$#&apos;, $date, $dt)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach ($dt AS $k =&gt; $v) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (is_int($k)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; unset($dt[$k]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!checkdate($dt[&apos;month&apos;], $dt[&apos;day&apos;], $dt[&apos;year&apos;])) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $dt[&apos;error_count&apos;] = 1;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $dt[&apos;error_count&apos;] = 1;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; $dt[&apos;errors&apos;] = array();
&#xA0; &#xA0; &#xA0; &#xA0; $dt[&apos;fraction&apos;] = &apos;&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; $dt[&apos;warning_count&apos;] = 0;
&#xA0; &#xA0; &#xA0; &#xA0; $dt[&apos;warnings&apos;] = array();
&#xA0; &#xA0; &#xA0; &#xA0; $dt[&apos;is_localtime&apos;] = 0;
&#xA0; &#xA0; &#xA0; &#xA0; $dt[&apos;zone_type&apos;] = 0;
&#xA0; &#xA0; &#xA0; &#xA0; $dt[&apos;zone&apos;] = 0;
&#xA0; &#xA0; &#xA0; &#xA0; $dt[&apos;is_dst&apos;] = &apos;&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; return $dt;
&#xA0; &#xA0; }

}
?>
```


Not my invention though. I found it here: http://stackoverflow.com/questions/6668223/php-date-parse-from-format-alternative-in-php-5-2

Thought this might be a good place to keep a copy in case someone stumbles upon the same problem facing outdated PHP versions on customer servers ....

  

#

[Official documentation page](https://www.php.net/manual/en/function.date-parse-from-format.php)

**[To root](/README.md)**