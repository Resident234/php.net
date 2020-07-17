# session_status



Maybe depending on PHP settings, but if return values are not the above, then go for this:<br>_DISABLED = 0<br>_NONE = 1<br>_ACTIVE = 2  

#

Universal function for checking session status.<br><br>

```
<?php
/**
 * @return bool
 */
function is_session_started()
{
    if ( php_sapi_name() !== 'cli' ) {
        if ( version_compare(phpversion(), '5.4.0', '>=') ) {
            return session_status() === PHP_SESSION_ACTIVE ? TRUE : FALSE;
        } else {
            return session_id() === '' ? FALSE : TRUE;
        }
    }
    return FALSE;
}

// Example
if ( is_session_started() === FALSE ) session_start();
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.session-status.php)

**[To root](/README.md)**