# pcntl_fork



"Fatal Error" has always been the bane of my world because there is no way to capture and handle the condition in PHP. My team builds almost everything in PHP in order to leverage our core library of code, so it was of the essence to find a solution for this problem of scripts bombing unrecoverably and us never knowing about it.<br><br>One of our background automation systems creates a "task queue" of sorts and for each task in the queue, a PHP module is include()ed to handle the task. Sometimes however a poorly behaved module will nuke with a Fatal Error and take out the parent script with it.<br><br>I decided to try to use pcntl_fork() to isolate the task module from the parent code, and it seems to work: a Fatal Error generated within the module makes the child task bomb, and the waiting parent can simply catch the return code from the child and track/alert us to the problem as needed. <br><br>Naturally something similar could be done if I wanted to simply exec() the module and check the output, but then I would not have the benefit of the stateful environment that the parent script has so carefully prepared. This allows me to keep the child process within the context of the parent&apos;s running environment and not suffer the consequences of Fatal Errors stopping the task queue from continuing to process.<br><br>Here is fork_n_wait.php for your amusement:<br><br>

```
<?php

if (! function_exists('pcntl_fork')) die('PCNTL functions not available on this PHP installation');

for ($x = 1; $x < 5; $x++) {
   switch ($pid = pcntl_fork()) {
      case -1:
         // @fail
         die('Fork failed');
         break;

      case 0:
         // @child: Include() misbehaving code here
         print "FORK: Child #{$x} preparing to nuke...\n";
         generate_fatal_error(); // Undefined function
         break;

      default:
         // @parent
         print "FORK: Parent, letting the child run amok...\n";
         pcntl_waitpid($pid, $status);
         break;
   }
}

print "Done! :^)\n\n";
?>
```
<br><br>Which outputs:<br>php -q fork_n_wait.php<br>FORK: Child #1 preparing to nuke...<br>PHP Fatal error:  Call to undefined function generate_fatal_error() in ~fork_n_wait.php on line 16<br>FORK: Parent, letting the child run amok...<br>FORK: Child #2 preparing to nuke...<br>PHP Fatal error:  Call to undefined function generate_fatal_error() in ~/fork_n_wait.php on line 16<br>FORK: Parent, letting the child run amok...<br>FORK: Child #3 preparing to nuke...<br>PHP Fatal error:  Call to undefined function generate_fatal_error() in ~/fork_n_wait.php on line 16<br>FORK: Parent, letting the child run amok...<br>FORK: Child #4 preparing to nuke...<br>PHP Fatal error:  Call to undefined function generate_fatal_error() in ~/fork_n_wait.php on line 16<br>FORK: Parent, letting the child run amok...<br>Done! :^)  

#

I just thought of contributing to this awesome community and hope this can be of use to someone. Although PHP provides threaded options, and multi curl handles that run in parallel, I managed to bash out a solution to run each function as it&apos;s own process for non-threaded versions of PHP.<br><br>Usage:  #!/usr/bin/php<br>Usage: php -f /path/to/file<br><br>#!/usr/bin/php<br>

```
<?php
function fork_process($options) 
{
    $shared_memory_monitor = shmop_open(ftok(__FILE__, chr(0)), "c", 0644, count($options['process']));
    $shared_memory_ids = (object) array();
    for ($i = 1; $i <= count($options['process']); $i++) 
    {
        $shared_memory_ids->$i = shmop_open(ftok(__FILE__, chr($i)), "c", 0644, $options['size']);
    }
    for ($i = 1; $i <= count($options['process']); $i++) 
    { 
        $pid = pcntl_fork(); 
        if (!$pid) 
        { 
            if($i==1)
                usleep(100000);
            $shared_memory_data = $options['process'][$i - 1]();
            shmop_write($shared_memory_ids->$i, $shared_memory_data, 0);
            shmop_write($shared_memory_monitor, "1", $i-1);
            exit($i); 
        } 
    } 
    while (pcntl_waitpid(0, $status) != -1) 
    { 
        if(shmop_read($shared_memory_monitor, 0, count($options['process'])) == str_repeat("1", count($options['process'])))
        {
            $result = array();
            foreach($shared_memory_ids as $key=>$value)
            {
                $result[$key-1] = shmop_read($shared_memory_ids->$key, 0, $options['size']);
                shmop_delete($shared_memory_ids->$key);
            }
            shmop_delete($shared_memory_monitor);
            $options['callback']($result);
        }    
    } 
}

// Create shared memory block of size 1M for each function.
$options['size'] = pow(1024,2); 

// Define 2 functions to run as its own process.
$options['process'][0] = function()
{
    // Whatever you need goes here...
    // If you need the results, return its value.
    // Eg: Long running proccess 1
    sleep(1);
    return 'Hello ';
};
$options['process'][1] = function()
{
    // Whatever you need goes here...
    // If you need the results, return its value.
    // Eg:
    // Eg: Long running proccess 2
    sleep(1);
    return 'World!';
};
$options['callback'] = function($result)
{
    // $results is an array of return values...
    // $result[0] for $options['process'][0] &amp;
    // $result[1] for $options['process'][1] &amp;
    // Eg:
    echo $result[0].$result[1]."\n";    
};
fork_process($options);

?>
```
<br><br>If you&apos;d like to get the results back from a webpage, use exec(). Eg: echo exec(&apos;php -f /path/to/file&apos;);<br><br>Continue hacking! :)  

#

Its been easy to fork process with pcntl_fork.. but how can we control or process further once all child processes gets completed.. here is the way we can do that...<br><br>

```
<?php
for ($i = 1; $i <= 5; ++$i) {
        $pid = pcntl_fork();

        if (!$pid) {
            sleep(1);
            print "In child $i\n";
            exit($i);
        }
    }

    while (pcntl_waitpid(0, $status) != -1) {
        $status = pcntl_wexitstatus($status);
        echo "Child $status completed\n";
    }
?>
```
  

#

The reason for the MySQL "Lost Connection during query" issue when forking is the fact that the child process inherits the parent&apos;s database connection. When the child exits, the connection is closed. If the parent is performing a query at this very moment, it is doing it on an already closed connection, hence the error.<br><br>An easy way to avoid this is to create a new database connection in parent immediately after forking. Don&apos;t forget to force a new connection by passing true in the 4th argument of mysql_connect():<br><br>

```
<?php
// Create the MySQL connection
$db = mysql_connect($server, $username, $password);

$pid = pcntl_fork();
             
if ( $pid == -1 ) {        
    // Fork failed            
    exit(1);
} else if ( $pid ) {
    // We are the parent
    // Can no longer use $db because it will be closed by the child
    // Instead, make a new MySQL connection for ourselves to work with
    $db = mysql_connect($server, $username, $password, true);
} else {
    // We are the child
    // Do something with the inherited connection here
    // It will get closed upon exit
    exit(0);
?>
```
<br><br>This way, the child will inherit the old connection, will work on it and will close upon exit. The parent won&apos;t care, because it will open a new connection for itself immediately after forking.<br><br>Hope this helps.  

#

If you want to execute some code after your php page has been returned to the user. Try something like this -<br><br>

```
<?php
function index()
{
        function shutdown() {
            posix_kill(posix_getpid(), SIGHUP);
        }

        // Do some initial processing

        echo("Hello World");

        // Switch over to daemon mode.

        if ($pid = pcntl_fork())
            return;     // Parent

        ob_end_clean(); // Discard the output buffer and close

        fclose(STDIN);  // Close all of the standard
        fclose(STDOUT); // file descriptors as we
        fclose(STDERR); // are running as a daemon.

        register_shutdown_function('shutdown');

        if (posix_setsid() < 0)
            return;

        if ($pid = pcntl_fork())
            return;     // Parent

        // Now running as a daemon. This process will even survive
        // an apachectl stop.

        sleep(10);

        $fp = fopen("/tmp/sdf123", "w");
        fprintf($fp, "PID = %s\n", posix_getpid());
        fclose($fp);

        return;
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.pcntl-fork.php)

**[To root](/README.md)**