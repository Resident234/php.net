# proc_terminate



/bin/sh -c CMD will fork sh and then exec CMD.<br>/bin/sh -c exec CMD will NOT fork and only executes CMD.<br><br>Therefore, you can get rid of this hack by prefixing your command to "exec bla bla bla".  

#

As explained in http://bugs.php.net/bug.php?id=39992, proc_terminate() leaves children of the child process running. In my application, these children often have infinite loops, so I need a sure way to kill processes created with proc_open(). When I call proc_terminate(), the /bin/sh process is killed, but the child with the infinite loop is left running. <br><br>Until proc_terminate() gets fixed, I would not recommend using it. Instead, my solution is to:<br>1) call proc_get_status() to get the parent pid (ppid) of the process I want to kill. <br>2) use ps to get all pids that have that ppid as their parent pid<br>3) use posix_kill() to send the SIGKILL (9) signal to each of those child pids<br>4) call proc_close() on process resource<br><br>

```
<?php
$descriptorspec = array(
0 =&gt; array(&apos;pipe&apos;, &apos;r&apos;),  // stdin is a pipe that the child will read from
1 =&gt; array(&apos;pipe&apos;, &apos;w&apos;),  // stdout is a pipe that the child will write to
2 =&gt; array(&apos;pipe&apos;, &apos;w&apos;)   // stderr is a pipe the child will write to
);
$process = proc_open(&apos;bad_program&apos;, $descriptorspec, $pipes);
if(!is_resource($process)) {
    throw new Exception(&apos;bad_program could not be started.&apos;);
}
//pass some input to the program
fwrite($pipes[0], $lots_of_data);
//close stdin. By closing stdin, the program should exit
//after it finishes processing the input
fclose($pipes[0]);

//do some other stuff ... the process will probably still be running
//if we check on it right away

$status = proc_get_status($process);
if($status[&apos;running&apos;] == true) { //process ran too long, kill it
    //close all pipes that are still open
    fclose($pipes[1]); //stdout
    fclose($pipes[2]); //stderr
    //get the parent pid of the process we want to kill
    $ppid = $status[&apos;pid&apos;];
    //use ps to get all the children of this process, and kill them
    $pids = preg_split(&apos;/\s+/&apos;, `ps -o pid --no-heading --ppid $ppid`);
    foreach($pids as $pid) {
        if(is_numeric($pid)) {
            echo "Killing $pid\n";
            posix_kill($pid, 9); //9 is the SIGKILL signal
        }
    }
        
    proc_close($process);
}

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.proc-terminate.php)

**[To root](/README.md)**