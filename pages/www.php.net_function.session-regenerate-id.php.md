# session_regenerate_id



I wrote the current top voted comment on this and wanted to add something. The existing code from my previous comment generates it&apos;s nonces in an insecure way-<br><br>

```
<?php
$_SESSION['nonce'] = md5(microtime(true));
?>
```


Since "microtime" is predictable it makes brute forcing the nonce much easier. A better option would be something that utilizes randomness, such as-



```
<?php
bin2hex(openssl_random_pseudo_bytes(32))
?>
```
  

---

I wrote the following code for a project I&apos;m working on- it attempts to resolve the regenerate issue, as well as deal with a couple of other session related things.<br><br>I tried to make it a little more generic and usable (for instance, in the full version it throws different types of exceptions for the different types of session issues), so hopefully someone might find it useful.<br><br>

```
<?php
function regenerateSession($reload = false)
{
    // This token is used by forms to prevent cross site forgery attempts
    if(!isset($_SESSION['nonce']) || $reload)
        $_SESSION['nonce'] = md5(microtime(true));

    if(!isset($_SESSION['IPaddress']) || $reload)
        $_SESSION['IPaddress'] = $_SERVER['REMOTE_ADDR'];

    if(!isset($_SESSION['userAgent']) || $reload)
        $_SESSION['userAgent'] = $_SERVER['HTTP_USER_AGENT'];

    //$_SESSION['user_id'] = $this->user->getId();

    // Set current session to expire in 1 minute
    $_SESSION['OBSOLETE'] = true;
    $_SESSION['EXPIRES'] = time() + 60;

    // Create new session without destroying the old one
    session_regenerate_id(false);

    // Grab current session ID and close both sessions to allow other scripts to use them
    $newSession = session_id();
    session_write_close();

    // Set session ID to the new one, and start it back up again
    session_id($newSession);
    session_start();

    // Don't want this one to expire
    unset($_SESSION['OBSOLETE']);
    unset($_SESSION['EXPIRES']);
}

function checkSession()
{
    try{
        if($_SESSION['OBSOLETE'] &amp;&amp; ($_SESSION['EXPIRES'] < time()))
            throw new Exception('Attempt to use expired session.');

        if(!is_numeric($_SESSION['user_id']))
            throw new Exception('No session started.');

        if($_SESSION['IPaddress'] != $_SERVER['REMOTE_ADDR'])
            throw new Exception('IP Address mixmatch (possible session hijacking attempt).');

        if($_SESSION['userAgent'] != $_SERVER['HTTP_USER_AGENT'])
            throw new Exception('Useragent mixmatch (possible session hijacking attempt).');

        if(!$this->loadUser($_SESSION['user_id']))
            throw new Exception('Attempted to log in user that does not exist with ID: ' . $_SESSION['user_id']);

        if(!$_SESSION['OBSOLETE'] &amp;&amp; mt_rand(1, 100) == 1)
        {
            $this->regenerateSession();
        }

        return true;

    }catch(Exception $e){
        return false;
    }
}

?>
```
  

---

In PHP 5.6 (and probably older versions), session_regenerate_id(true) do not trigger a read() call to the session handler for the new session id. <br><br>In PHP 7, read() is triggered during session_regenerate_id(true). Nice to know when working with custom session handlers.  

---

In a previous note, php at 5mm de describes how to prevent session hijacking by<br>ensuring that the session id provided matches the HTTP_USER_AGENT and REMOTE_ADDR fields that were present when the session id was first issued.  It should be noted that HTTP_USER_AGENT is supplied by the client, and so can be easily modified by a malicious user.  Also, the client IP addresses can be spoofed, although that&apos;s a bit more difficult.  Care should be taken when relying on the session for authentication.  

---

[Official documentation page](https://www.php.net/manual/en/function.session-regenerate-id.php)

**[To root](/README.md)**