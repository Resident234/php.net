# date_parse_from_format



For use in Versions prior V5.3:<br><br>

```
<?php
if (!function_exists('date_parse_from_format')) {

    function date_parse_from_format($format, $date) {
        // reverse engineer date formats
        $keys = array(
            'Y' => array('year', '\d{4}'),
            'y' => array('year', '\d{2}'),
            'm' => array('month', '\d{2}'),
            'n' => array('month', '\d{1,2}'),
            'M' => array('month', '[A-Z][a-z]{3}'),
            'F' => array('month', '[A-Z][a-z]{2,8}'),
            'd' => array('day', '\d{2}'),
            'j' => array('day', '\d{1,2}'),
            'D' => array('day', '[A-Z][a-z]{2}'),
            'l' => array('day', '[A-Z][a-z]{6,9}'),
            'u' => array('hour', '\d{1,6}'),
            'h' => array('hour', '\d{2}'),
            'H' => array('hour', '\d{2}'),
            'g' => array('hour', '\d{1,2}'),
            'G' => array('hour', '\d{1,2}'),
            'i' => array('minute', '\d{2}'),
            's' => array('second', '\d{2}')
        );

        // convert format string to regex
        $regex = '';
        $chars = str_split($format);
        foreach ($chars AS $n => $char) {
            $lastChar = isset($chars[$n - 1]) ? $chars[$n - 1] : '';
            $skipCurrent = '\\' == $lastChar;
            if (!$skipCurrent &amp;&amp; isset($keys[$char])) {
                $regex .= '(?P&lt;' . $keys[$char][0] . '&gt;' . $keys[$char][1] . ')';
            } else if ('\\' == $char) {
                $regex .= $char;
            } else {
                $regex .= preg_quote($char);
            }
        }

        $dt = array();
        $dt['error_count'] = 0;
        // now try to match it
        if (preg_match('#^' . $regex . '$#', $date, $dt)) {
            foreach ($dt AS $k => $v) {
                if (is_int($k)) {
                    unset($dt[$k]);
                }
            }
            if (!checkdate($dt['month'], $dt['day'], $dt['year'])) {
                $dt['error_count'] = 1;
            }
        } else {
            $dt['error_count'] = 1;
        }
        $dt['errors'] = array();
        $dt['fraction'] = '';
        $dt['warning_count'] = 0;
        $dt['warnings'] = array();
        $dt['is_localtime'] = 0;
        $dt['zone_type'] = 0;
        $dt['zone'] = 0;
        $dt['is_dst'] = '';
        return $dt;
    }

}
?>
```


Not my invention though. I found it here: http://stackoverflow.com/questions/6668223/?>
```
date-parse-from-format-alternative-in

```
<??>
```
5-2<br><br>Thought this might be a good place to keep a copy in case someone stumbles upon the same problem facing outdated PHP versions on customer servers ....  

#

[Official documentation page](https://www.php.net/manual/en/function.date-parse-from-format.php)

**[To root](/README.md)**