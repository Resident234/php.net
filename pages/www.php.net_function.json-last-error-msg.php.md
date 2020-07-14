# json_last_error_msg



Here&apos;s an updated version of the function:<br><br>

```
<?php
    if (!function_exists(&apos;json_last_error_msg&apos;)) {
        function json_last_error_msg() {
            static $ERRORS = array(
                JSON_ERROR_NONE =&gt; &apos;No error&apos;,
                JSON_ERROR_DEPTH =&gt; &apos;Maximum stack depth exceeded&apos;,
                JSON_ERROR_STATE_MISMATCH =&gt; &apos;State mismatch (invalid or malformed JSON)&apos;,
                JSON_ERROR_CTRL_CHAR =&gt; &apos;Control character error, possibly incorrectly encoded&apos;,
                JSON_ERROR_SYNTAX =&gt; &apos;Syntax error&apos;,
                JSON_ERROR_UTF8 =&gt; &apos;Malformed UTF-8 characters, possibly incorrectly encoded&apos;
            );

            $error = json_last_error();
            return isset($ERRORS[$error]) ? $ERRORS[$error] : &apos;Unknown error&apos;;
        }
    }
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.json-last-error-msg.php)

**[To root](/README.md)**