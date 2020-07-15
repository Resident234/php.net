# proc_open



The call works as should. No bugs.<br>But. In most cases you won&apos;t able to work with pipes in blocking mode.<br>When your output pipe (process&apos; input one, $pipes[0]) is blocking, there is a case, when you and the process are blocked on output.<br>When your input pipe (process&apos; output one, $pipes[1]) is blocking, there is a case, when you and the process both are blocked on own input.<br>So you should switch pipes into NONBLOCKING mode (stream_set_blocking).<br>Then, there is a case, when you&apos;re not able to read anything (fread($pipes[1],...) == "") either write (fwrite($pipes[0],...) == 0). In this case, you better check the process is alive (proc_get_status) and if it still is - wait for some time (stream_select). The situation is truly asynchronous, the process may be busy working, processing your data.<br>Using shell effectively makes not possible to know whether the command is exists - proc_open always returns valid resource. You may even write some data into it (into shell, actually). But eventually it will terminate, so check the process status regularly.<br>I would advice not using mkfifo-pipes, because filesystem fifo-pipe (mkfifo) blocks open/fopen call (!!!) until somebody opens other side (unix-related behavior). In case the pipe is opened not by shell and the command is crashed or is not exists you will be blocked forever.  

#

Note that when you call an external script and retrieve large amounts of data from STDOUT and STDERR, you may need to retrieve from both alternately in non-blocking mode (with appropriate pauses if no data is retrieved), so that your PHP script doesn&apos;t lock up. This can happen if you waiting on activity on one pipe while the external script is waiting for you to empty the other, e.g:<br><br>

```
<?php
$read_output = $read_error = false;
$buffer_len  = $prev_buffer_len = 0; 
$ms          = 10;
$output      = '';
$read_output = true;
$error       = '';
$read_error  = true;
stream_set_blocking($pipes[1], 0);
stream_set_blocking($pipes[2], 0);

// dual reading of STDOUT and STDERR stops one full pipe blocking the other, because the external script is waiting
while ($read_error != false or $read_output != false)
{
    if ($read_output != false)
    {
        if(feof($pipes[1])) 
        {
            fclose($pipes[1]);
            $read_output = false;
        }
        else 
        {
            $str = fgets($pipes[1], 1024);
            $len = strlen($str);
            if ($len)
            {
                $output .= $str; 
                $buffer_len += $len;
            }
        }
    }
    
    if ($read_error != false)
    {
        if(feof($pipes[2])) 
        {
            fclose($pipes[2]);
            $read_error = false;
        }
        else 
        {
            $str = fgets($pipes[2], 1024);
            $len = strlen($str);
            if ($len)
            {
                $error .= $str; 
                $buffer_len += $len;
            }
        }
    }
    
    if ($buffer_len > $prev_buffer_len)
    {
        $prev_buffer_len = $buffer_len;
        $ms = 10;
    }
    else 
    {
        usleep($ms * 1000); // sleep for $ms milliseconds
        if ($ms < 160)
        {
            $ms = $ms * 2;
        }
    }
}
        
return proc_close($process);
?>
```
  

#

Interestingly enough, it seems you actually have to store the return value in order for your streams to exist. You can&apos;t throw it away.<br><br>In other words, this works:<br><br>

```
<?php
$proc=proc_open("echo foo",
  array(
    array("pipe","r"),
    array("pipe","w"),
    array("pipe","w")
  ),
  $pipes);
print stream_get_contents($pipes[1]);
?>
```


prints:
foo

but this doesn't work:



```
<?php
proc_open("echo foo",
  array(
    array("pipe","r"),
    array("pipe","w"),
    array("pipe","w")
  ),
  $pipes);
print stream_get_contents($pipes[1]);
?>
```
<br><br>outputs:<br>Warning: stream_get_contents(): &lt;n&gt; is not a valid stream resource in Command line code on line 1<br><br>The only difference is that in the second case we don&apos;t save the output of proc_open to a variable.  

#

It took me a long time (and three consecutive projects) to figure this out.  Because popen() and proc_open() return valid processes even when the command failed it&apos;s awkward to determine when it really has failed if you&apos;re opening a non-interactive process like "sendmail -t".<br><br>I had previously guess that reading from STDERR immediately after starting the process would work, and it does... but when the command is successful PHP just hangs because STDERR is empty and it&apos;s waiting for data to be written to it.<br><br>The solution is a simple stream_set_blocking($pipes[2], 0) immediately after calling proc_open().<br><br>

```
<?php

    $this->_proc = proc_open($command, $descriptorSpec, $pipes);
    stream_set_blocking($pipes[2], 0);
    if ($err = stream_get_contents($pipes[2]))
    {
      throw new Swift_Transport_TransportException(
        'Process could not be started [' . $err . ']'
        );
    }

?>
```
<br><br>If the process is opened successfully $pipes[2] will be empty, but if it failed the bash/sh error will be in it.<br><br>Finally I can drop all my "workaround" error checking.<br><br>I realise this solution is obvious and I&apos;m not sure how it took me 18 months to figure it out, but hopefully this will help someone else.<br><br>NOTE: Make sure your descriptorSpec has ( 2 =&gt; array(&apos;pipe&apos;, &apos;w&apos;)) for this to work.  

#

It seems that stream_get_contents() on STDOUT blocks infinitly under Windows when STDERR is filled under some circumstances.<br><br>The trick is to open STDERR in append mode ("a"), then this will work, too.<br><br>

```
<?php
$descriptorspec = array(
    0 => array('pipe', 'r'), // stdin
    1 => array('pipe', 'w'), // stdout
    2 => array('pipe', 'a') // stderr
);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.proc-open.php)

**[To root](/README.md)**