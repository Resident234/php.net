# session_start



If you want to handle sessions with a class, I wrote this little class:<br><br>

```
<?php

/*
    Use the static method getInstance to get the object.
*/

class Session
{
    const SESSION_STARTED = TRUE;
    const SESSION_NOT_STARTED = FALSE;
    
    // The state of the session
    private $sessionState = self::SESSION_NOT_STARTED;
    
    // THE only instance of the class
    private static $instance;
    
    
    private function __construct() {}
    
    
    /**
    *    Returns THE instance of 'Session'.
    *    The session is automatically initialized if it wasn't.
    *    
    *    @return    object
    **/
    
    public static function getInstance()
    {
        if ( !isset(self::$instance))
        {
            self::$instance = new self;
        }
        
        self::$instance->startSession();
        
        return self::$instance;
    }
    
    
    /**
    *    (Re)starts the session.
    *    
    *    @return    bool    TRUE if the session has been initialized, else FALSE.
    **/
    
    public function startSession()
    {
        if ( $this->sessionState == self::SESSION_NOT_STARTED )
        {
            $this->sessionState = session_start();
        }
        
        return $this->sessionState;
    }
    
    
    /**
    *    Stores datas in the session.
    *    Example: $instance->foo = 'bar';
    *    
    *    @param    name    Name of the datas.
    *    @param    value    Your datas.
    *    @return    void
    **/
    
    public function __set( $name , $value )
    {
        $_SESSION[$name] = $value;
    }
    
    
    /**
    *    Gets datas from the session.
    *    Example: echo $instance->foo;
    *    
    *    @param    name    Name of the datas to get.
    *    @return    mixed    Datas stored in session.
    **/
    
    public function __get( $name )
    {
        if ( isset($_SESSION[$name]))
        {
            return $_SESSION[$name];
        }
    }
    
    
    public function __isset( $name )
    {
        return isset($_SESSION[$name]);
    }
    
    
    public function __unset( $name )
    {
        unset( $_SESSION[$name] );
    }
    
    
    /**
    *    Destroys the current session.
    *    
    *    @return    bool    TRUE is session has been deleted, else FALSE.
    **/
    
    public function destroy()
    {
        if ( $this->sessionState == self::SESSION_STARTED )
        {
            $this->sessionState = !session_destroy();
            unset( $_SESSION );
            
            return !$this->sessionState;
        }
        
        return FALSE;
    }
}

/*
    Examples:
*/

// We get the instance
$data = Session::getInstance();

// Let's store datas in the session
$data->nickname = 'Someone';
$data->age = 18;

// Let's display datas
printf( '&lt;p&gt;My name is %s and I\'m %d years old.&lt;/p&gt;' , $data->nickname , $data->age );

/*
    It will display:
    
    Array
    (
        [nickname] => Someone
        [age] => 18
    )
*/

printf( '&lt;pre&gt;%s&lt;/pre&gt;' , print_r( $_SESSION , TRUE ));

// TRUE
var_dump( isset( $data->nickname ));

// We destroy the session
$data->destroy();

// FALSE
var_dump( isset( $data->nickname ));

?>
```
<br><br>I prefer using this class instead of using directly the array $_SESSION.  

#

As others have noted, PHP&apos;s session handler is blocking. When one of your scripts calls session_start(), any other script that also calls session_start() with the same session ID will sleep until the first script closes the session.<br><br>A common workaround to this is call session_start() and session_write_close() each time you want to update the session.<br><br>The problem with this, is that each time you call session_start(), PHP prints a duplicate copy of the session cookie to the HTTP response header. Do this enough times (as you might do in a long-running script), and the response header can get so large that it causes web servers &amp; browsers to crash or reject your response as malformed.<br><br>This error has been reported to PHP HQ, but they&apos;ve marked it "Won&apos;t fix" because they say you&apos;re not supposed to open and close the session during a single script like this. https://bugs.php.net/bug.php?id=31455<br><br>As a workaround, I&apos;ve written a function that uses headers_list() and header_remove() to clear out the duplicate cookies. It&apos;s interesting to note that even on requests when PHP sends duplicate session cookies, headers_list() still only lists one copy of the session cookie. Nonetheless, calling header_remove() removes all the duplicate copies.<br><br>

```
<?php
/**
 * Every time you call session_start(), PHP adds another
 * identical session cookie to the response header. Do this
 * enough times, and your response header becomes big enough
 * to choke the web server.
 *
 * This method clears out the duplicate session cookies. You can
 * call it after each time you've called session_start(), or call it
 * just before you send your headers.
 */
function clear_duplicate_cookies() {
    // If headers have already been sent, there's nothing we can do
    if (headers_sent()) {
        return;
    }

    $cookies = array();
    foreach (headers_list() as $header) {
        // Identify cookie headers
        if (strpos($header, 'Set-Cookie:') === 0) {
            $cookies[] = $header;
        }
    }
    // Removes all cookie headers, including duplicates
    header_remove('Set-Cookie');

    // Restore one copy of each cookie
    foreach(array_unique($cookies) as $cookie) {
        header($cookie, false);
    }
}
?>
```
  

#

The constant SID would always be &apos;&apos; (an empty string) if directive session.use_trans_sid in php ini file is set to 0. <br><br>So remember to set session.use_trans_sid to 1 and restart your server before you use SID in your php script.  

#

PHP locks the session file until it is closed. If you have 2 scripts using the same session (i.e. from the same user) then the 2nd script will not finish its call to session_start() until the first script finishes execution.<br><br>If you have scripts that run for more than a second and users may be making more than 1 request at a time then it is worth calling session_write_close() as soon as you&apos;ve finished writing session data.<br><br>

```
<?php
// a lock is places on the session, so other scripts will have to wait
session_start();

// do all your writing to $_SESSION
$_SESSION['a'] = 1;

// $_SESSION can still be read, but writing will not update the session.
// the lock is removed and other scripts can now read the session
session_write_close();

do_something_slow();
?>
```


Found this out from http://konrness.com/php5/how-to-prevent-blocking

```
<??>
```
requests/  

#

If you are using a custom session handler via session_set_save_handler() then calling session_start() in PHP 7.1 you might see an error like this:<br>session_start(): Failed to read session data: user (path: /var/lib/php/session) in ...<br><br>As of this writing, it seems to be happening in PHP 7.1, and things look OK in PHP7.0. <br><br>It is also hard to track down because if a session already exists for this id (maybe created by an earlier version of PHP), it will not trigger this issue because the $session_data will not be null.<br><br>The fix is simple... you just need to check for &apos;null&apos; during your read function:<br><br>

```
<?php

function read($id)
{
  //... pull the data out of the DB, off the disk, memcache, etc
  $session_data = getSessionDataFromSomewhere($id);
  
  //check to see if $session_data is null before returning (CRITICAL)
  if(is_null($session_data))
  {
    $session_data = '';  //use empty string instead of null!
  }

  return $session_data;
}

?>
```
  

#

When you have an import script that takes long to execute, the browser seem to lock up and you cannot access the website anymore. this is because a request is reading and locking the session file to prevent corruption.<br><br>you can either <br>- use a different session handler with session_set_save_handler()<br>- use session_write_close() in the import script as soon you don&apos;t need session anymore (best moment is just before the long during part takes place), you can session_start when ever you want and as many times you like if your import script requires session variables changed. <br><br>example<br>

```
<?php
session_start(); //initiate / open session
$_SESSION['count'] = 0; // store something in the session
session_write_close(); //now close it, 
# from here every other script can be run (and makes it seem like multitasking)
for($i=0; $i&lt;=100; $i++){ //do 100 cycles
    session_start(); //open the session again for editing a variable
    $_SESSION['count'] += 1; //change variable
    session_write_close(); //now close the session again!
    sleep(2); //every cycle sleep two seconds, or do a heavy task
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.session-start.php)

**[To root](/README.md)**