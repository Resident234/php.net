# json_last_error



While this can obviously change between versions, the current error codes are as follows:<br><br>0 = JSON_ERROR_NONE<br>1 = JSON_ERROR_DEPTH<br>2 = JSON_ERROR_STATE_MISMATCH<br>3 = JSON_ERROR_CTRL_CHAR<br>4 = JSON_ERROR_SYNTAX<br>5 = JSON_ERROR_UTF8<br><br>I&apos;m only posting these for people who may be trying to understand why specific JSON files are not being decoded. Please do not hard-code these numbers into an error handler routine.  

#

I used this simple script, flicked from StackOverflow to escape from the function failing:<br><br>

```
<?php
    function utf8ize($d) {
        if (is_array($d)) {
            foreach ($d as $k =&gt; $v) {
                $d[$k] = utf8ize($v);
            }
        } else if (is_string ($d)) {
            return utf8_encode($d);
        }
        return $d;
    }
?>
```
<br><br>Cheers,<br>Praveen Kumar!  

#

when json_decode a empty string, PHP7 will trigger an Syntax error:<br>

```
<?php<br>json_decode("");<br>var_dump(json_last_error(), json_last_error_msg());<br><br>// PHP 7<br>int(4)<br>string(12) "Syntax error"<br><br>//  PHP 5<br>int(0)<br>string(8) "No error"  

#

[Official documentation page](https://www.php.net/manual/en/function.json-last-error.php)

**[To root](/README.md)**