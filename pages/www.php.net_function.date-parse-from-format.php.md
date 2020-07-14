# date_parse_from_format



For use in Versions prior V5.3:<br><br>

```
<?php
if (!function_exists(&apos;date_parse_from_format&apos;)) {

    function date_parse_from_format($format, $date) {
        // reverse engineer date formats
        $keys = array(
            &apos;Y&apos; =&gt; array(&apos;year&apos;, &apos;\d{4}&apos;),
            &apos;y&apos; =&gt; array(&apos;year&apos;, &apos;\d{2}&apos;),
            &apos;m&apos; =&gt; array(&apos;month&apos;, &apos;\d{2}&apos;),
            &apos;n&apos; =&gt; array(&apos;month&apos;, &apos;\d{1,2}&apos;),
            &apos;M&apos; =&gt; array(&apos;month&apos;, &apos;[A-Z][a-z]{3}&apos;),
            &apos;F&apos; =&gt; array(&apos;month&apos;, &apos;[A-Z][a-z]{2,8}&apos;),
            &apos;d&apos; =&gt; array(&apos;day&apos;, &apos;\d{2}&apos;),
            &apos;j&apos; =&gt; array(&apos;day&apos;, &apos;\d{1,2}&apos;),
            &apos;D&apos; =&gt; array(&apos;day&apos;, &apos;[A-Z][a-z]{2}&apos;),
            &apos;l&apos; =&gt; array(&apos;day&apos;, &apos;[A-Z][a-z]{6,9}&apos;),
            &apos;u&apos; =&gt; array(&apos;hour&apos;, &apos;\d{1,6}&apos;),
            &apos;h&apos; =&gt; array(&apos;hour&apos;, &apos;\d{2}&apos;),
            &apos;H&apos; =&gt; array(&apos;hour&apos;, &apos;\d{2}&apos;),
            &apos;g&apos; =&gt; array(&apos;hour&apos;, &apos;\d{1,2}&apos;),
            &apos;G&apos; =&gt; array(&apos;hour&apos;, &apos;\d{1,2}&apos;),
            &apos;i&apos; =&gt; array(&apos;minute&apos;, &apos;\d{2}&apos;),
            &apos;s&apos; =&gt; array(&apos;second&apos;, &apos;\d{2}&apos;)
        );

        // convert format string to regex
        $regex = &apos;&apos;;
        $chars = str_split($format);
        foreach ($chars AS $n =&gt; $char) {
            $lastChar = isset($chars[$n - 1]) ? $chars[$n - 1] : &apos;&apos;;
            $skipCurrent = &apos;\\&apos; == $lastChar;
            if (!$skipCurrent &amp;&amp; isset($keys[$char])) {
                $regex .= &apos;(?P&lt;&apos; . $keys[$char][0] . &apos;&gt;&apos; . $keys[$char][1] . &apos;)&apos;;
            } else if (&apos;\\&apos; == $char) {
                $regex .= $char;
            } else {
                $regex .= preg_quote($char);
            }
        }

        $dt = array();
        $dt[&apos;error_count&apos;] = 0;
        // now try to match it
        if (preg_match(&apos;#^&apos; . $regex . &apos;$#&apos;, $date, $dt)) {
            foreach ($dt AS $k =&gt; $v) {
                if (is_int($k)) {
                    unset($dt[$k]);
                }
            }
            if (!checkdate($dt[&apos;month&apos;], $dt[&apos;day&apos;], $dt[&apos;year&apos;])) {
                $dt[&apos;error_count&apos;] = 1;
            }
        } else {
            $dt[&apos;error_count&apos;] = 1;
        }
        $dt[&apos;errors&apos;] = array();
        $dt[&apos;fraction&apos;] = &apos;&apos;;
        $dt[&apos;warning_count&apos;] = 0;
        $dt[&apos;warnings&apos;] = array();
        $dt[&apos;is_localtime&apos;] = 0;
        $dt[&apos;zone_type&apos;] = 0;
        $dt[&apos;zone&apos;] = 0;
        $dt[&apos;is_dst&apos;] = &apos;&apos;;
        return $dt;
    }

}
?>
```
<br><br>Not my invention though. I found it here: http://stackoverflow.com/questions/6668223/php-date-parse-from-format-alternative-in-php-5-2<br><br>Thought this might be a good place to keep a copy in case someone stumbles upon the same problem facing outdated PHP versions on customer servers ....  

#

[Official documentation page](https://www.php.net/manual/en/function.date-parse-from-format.php)

**[To root](/README.md)**