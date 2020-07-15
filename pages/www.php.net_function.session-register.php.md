# session_register



Below is a fix that may be included in older code to make it work with PHP6. <br>When needed it recreates the functions<br>- session_register()<br>- session_is_registered()<br>- session_unregister()<br><br>The functions inside the function fix_session_register() are only available  after fix_session_register() has run.<br>Therefore in PHP&lt;6 where there already is a session_register() nothing happens.<br><br>

```
<?php
// Fix for removed Session functions
function fix_session_register(){
    function session_register(){
        $args = func_get_args();
        foreach ($args as $key){
            $_SESSION[$key]=$GLOBALS[$key];
        }
    }
    function session_is_registered($key){
        return isset($_SESSION[$key]);
    }
    function session_unregister($key){
        unset($_SESSION[$key]);
    }
}
if (!function_exists('session_register')) fix_session_register();
?>
```
<br><br><br>[EDIT BY danbrown AT php DOT net: Bugfix provided by "dr3w" on 02-APR-2010: "its [sic] function_exists with an S at the end".]  

#

if you remove session_register() calls and replace with $_SESSION assignments, make sure that it wasn&apos;t being used in place of session_start(). If it was, you&apos;ll need to add a call to session_start() too, before you assign to $_SESSION.  

#

[Official documentation page](https://www.php.net/manual/en/function.session-register.php)

**[To root](/README.md)**