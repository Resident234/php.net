# Session Functions



When working on a project, I found a need to switch live sessions between two different pieces of software. The documentation to do this is scattered all around different sites, especially in comments sections rather than examples. One difficulty I encountered was the session save handler for one of the applications was set, whereas the other was not. Now, I didn&apos;t code in the function session_set_save_handler(), instead I utilize that once I&apos;m done with the function (manually), however this function could easily be extended to include that functionality. Basically, it is only overriding the system&apos;s default session save handler. To overcome this after you have used getSessionData(), just call session_write_close(), session_set_save_handler() with the appropriate values, then re-run session_name(), session_id() and session_start() with their appropriate values. If you don&apos;t know the session id, it&apos;s the string located in $_COOKIE[session_name], or $_REQUEST[session_name] if you are using trans_sid. [note: use caution with trusting data from $_REQUEST, if at all possible, use $_GET or $_POST instead depending on the page].<br><br>

```
<?php
function getSessionData ($session_name = &apos;PHPSESSID&apos;, $session_save_handler = &apos;files&apos;) {
    $session_data = array();
    # did we get told what the old session id was? we can&apos;t continue it without that info
    if (array_key_exists($session_name, $_COOKIE)) {
        # save current session id
        $session_id = $_COOKIE[$session_name];
        $old_session_id = session_id();
        
        # write and close current session
        session_write_close();
        
        # grab old save handler, and switch to files
        $old_session_save_handler = ini_get(&apos;session.save_handler&apos;);
        ini_set(&apos;session.save_handler&apos;, $session_save_handler);
        
        # now we can switch the session over, capturing the old session name
        $old_session_name = session_name($session_name);
        session_id($session_id);
        session_start();
        
        # get the desired session data
        $session_data = $_SESSION;
        
        # close this session, switch back to the original handler, then restart the old session
        session_write_close();
        ini_set(&apos;session.save_handler&apos;, $old_session_save_handler);
        session_name($old_session_name);
        session_id($old_session_id);
        session_start();
    }
    
    # now return the data we just retrieved
    return $session_data;
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/ref.session.php)

**[To root](/README.md)**