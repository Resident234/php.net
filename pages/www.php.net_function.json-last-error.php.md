# json_last_error



While this can obviously change between versions, the current error codes are as follows:<br><br>0 = JSON_ERROR_NONE<br>1 = JSON_ERROR_DEPTH<br>2 = JSON_ERROR_STATE_MISMATCH<br>3 = JSON_ERROR_CTRL_CHAR<br>4 = JSON_ERROR_SYNTAX<br>5 = JSON_ERROR_UTF8<br><br>I&apos;m only posting these for people who may be trying to understand why specific JSON files are not being decoded. Please do not hard-code these numbers into an error handler routine.  

---

I used this simple script, flicked from StackOverflow to escape from the function failing:<br><br>

```
<?php
    function utf8ize($d) {
        if (is_array($d)) {
            foreach ($d as $k => $v) {
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

---

when json_decode a empty string, PHP7 will trigger an Syntax error:<br>

```
<?php
json_decode("");
var_dump(json_last_error(), json_last_error_msg());

// PHP 7
int(4)
string(12) "Syntax error"

//  PHP 5
int(0)
string(8) "No error"?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.json-last-error.php)

**[To root](/README.md)**