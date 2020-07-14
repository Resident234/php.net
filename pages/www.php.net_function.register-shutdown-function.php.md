# register_shutdown_function



If your script exceeds the maximum execution time, and terminates thusly:<br><br>Fatal error: Maximum execution time of 20 seconds exceeded in - on line 12<br><br>The registered shutdown functions will still be executed.<br><br>I figured it was important that this be made clear!  

#

If you want to do something with files in function, that registered in register_shutdown_function(), use ABSOLUTE paths to files instead of relative. Because when script processing is complete current working directory chages to ServerRoot (see httpd.conf)  

#

You may get the idea to call debug_backtrace or debug_print_backtrace from inside a shutdown function, to trace where a fatal error occurred. Unfortunately, these functions will not work inside a shutdown function.  

#

If you register function that needs to be running last (for example, close database connection) - just register another shutdown function from shutdown function:<br>

```
<?php
function test1(){
  register_shutdown_function(&apos;test_last&apos;);
}

function test2(){/*...*/}
function test3(){/*...*/}
function test_last(){/*...*/}

register_shutdown_function(&apos;test1&apos;);
register_shutdown_function(&apos;test2&apos;);
register_shutdown_function(&apos;test3&apos;);
?>
```
<br><br>the script will call functions in correct order: test1, test2, test3, test_last  

#

You definitely need to be careful about using relative paths in after the shutdown function has been called, but the current working directory doesn&apos;t (necessarily) get changed to the web server&apos;s ServerRoot - I&apos;ve tested on two different servers and they both have their CWD changed to &apos;/&apos; (which isn&apos;t the ServerRoot).<br><br>This demonstrates the behaviour:<br><br>

```
<?php
function echocwd() { echo &apos;cwd: &apos;, getcwd(), "\n"; }

register_shutdown_function(&apos;echocwd&apos;);
echocwd() and exit;
?>
```
<br><br>Outputs:<br><br>cwd: /path/to/my/site/docroot/test<br>cwd: /<br><br>NB: CLI scripts are unaffected, and keep their CWD as the directory the script was called from.  

#

A lot of useful services may be delegated to this useful trigger.<br>It is very effective because it is executed at the end of the script but before any object destruction, so all instantiations are still alive.<br><br>Here&apos;s a simple shutdown events manager class which allows to manage either functions or static/dynamic methods, with an indefinite number of arguments without using any reflection, availing on a internal handling through func_get_args() and call_user_func_array() specific functions:<br><br>

```
<?php
// managing the shutdown callback events:
class shutdownScheduler {
    private $callbacks; // array to store user callbacks
    
    public function __construct() {
        $this-&gt;callbacks = array();
        register_shutdown_function(array($this, &apos;callRegisteredShutdown&apos;));
    }
    public function registerShutdownEvent() {
        $callback = func_get_args();
        
        if (empty($callback)) {
            trigger_error(&apos;No callback passed to &apos;.__FUNCTION__.&apos; method&apos;, E_USER_ERROR);
            return false;
        }
        if (!is_callable($callback[0])) {
            trigger_error(&apos;Invalid callback passed to the &apos;.__FUNCTION__.&apos; method&apos;, E_USER_ERROR);
            return false;
        }
        $this-&gt;callbacks[] = $callback;
        return true;
    }
    public function callRegisteredShutdown() {
        foreach ($this-&gt;callbacks as $arguments) {
            $callback = array_shift($arguments);
            call_user_func_array($callback, $arguments);
        }
    }
    // test methods:
    public function dynamicTest() {
        echo &apos;_REQUEST array is &apos;.count($_REQUEST).&apos; elements long.&lt;br /&gt;&apos;;
    }
    public static function staticTest() {
        echo &apos;_SERVER array is &apos;.count($_SERVER).&apos; elements long.&lt;br /&gt;&apos;;
    }
}
?>
```


A simple application:



```
<?php
// a generic function
function say($a = &apos;a generic greeting&apos;, $b = &apos;&apos;) {
    echo "Saying {$a} {$b}&lt;br /&gt;";
}

$scheduler = new shutdownScheduler();

// schedule a global scope function:
$scheduler-&gt;registerShutdownEvent(&apos;say&apos;, &apos;hello!&apos;);

// try to schedule a dyamic method:
$scheduler-&gt;registerShutdownEvent(array($scheduler, &apos;dynamicTest&apos;));
// try with a static call:
$scheduler-&gt;registerShutdownEvent(&apos;scheduler::staticTest&apos;);

?>
```
<br><br>It is easy to guess how to extend this example in a more complex context in which user defined functions and methods should be handled according to the priority depending on specific variables.<br><br>Hope it may help somebody.<br>Happy coding!  

#

When using php-fpm, fastcgi_finish_request() should be used instead of register_shutdown_function() and exit()<br><br>For example, under nginx and php-fpm 5.3+, this will make browsers wait 10 seconds to show output:<br><br>

```
<?php
    echo "You have to wait 10 seconds to see this.&lt;br&gt;";
    register_shutdown_function(&apos;shutdown&apos;);
    exit;
    function shutdown(){
        sleep(10);
        echo "Because exit() doesn&apos;t terminate php-fpm calls immediately.&lt;br&gt;";
    }
?>
```


This doesn&apos;t:



```
<?php
    echo "You can see this from the browser immediately.&lt;br&gt;";
    fastcgi_finish_request();
    sleep(10);
    echo "You can&apos;t see this form the browser.";
?>
```
  

#

register_shutdown_function seems to be immune to whatever value was set with set_time_limit or the max_execution_time value defined in php.ini. <br>

```
<?php
function asdf() {
    echo microtime(true) . &apos;&lt;br&gt;&apos;;
    sleep(1);
    echo microtime(true) . &apos;&lt;br&gt;&apos;;
    sleep(1);
    echo microtime(true) . &apos;&lt;br&gt;&apos;;
}
register_shutdown_function(&apos;asdf&apos;);
set_time_limit(1);

while(true) {}
?>
```
<br>The output is three lines.  

#

[Official documentation page](https://www.php.net/manual/en/function.register-shutdown-function.php)

**[To root](/README.md)**