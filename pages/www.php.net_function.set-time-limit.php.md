# set_time_limit



while setting the set_time_limit(), the duration of sleep() will be ignored in the execution time. The following illustrates:<br><br>

```
<?php

set_time_limit(20);

while ($i&lt;=10)
{
        echo "i=$i ";
        sleep(100);
        $i++;
}

?>
```
<br><br>Output:<br>i=0 i=1 i=2 i=3 i=4 i=5 i=6 i=7 i=8 i=9 i=10  

#

Both set_time_limit(...) and  ini_set(&apos;max_execution_time&apos;,...); won&apos;t count the time cost of sleep,file_get_contents,shell_exec,mysql_query etc, so i build this function my_background_exec(), to run static method/function in background/detached process and time is out kill it:<br><br>my_exec.php:<br>

```
<?php
function my_background_exec($function_name, $params, $str_requires, $timeout=600)
         {$map=array(&apos;"&apos;=&gt;&apos;\"&apos;, &apos;

```
<?php<br>function my_background_exec($function_name, $params, $str_requires, $timeout=600)<br>         {$map=array(&apos;"&apos;=&gt;&apos;\"&apos;, &apos;$&apos;=&gt;&apos;\$&apos;, &apos;`&apos;=&gt;&apos;\`&apos;, &apos;\\&apos;=&gt;&apos;\\\\&apos;, &apos;!&apos;=&gt;&apos;\!&apos;);<br>          $str_requires=strtr($str_requires, $map);<br>          $path_run=dirname($_SERVER[&apos;SCRIPT_FILENAME&apos;]);<br>          $my_target_exec="/usr/bin/php -r \"chdir(&apos;{$path_run}&apos;);{$str_requires} \\\$params=json_decode(file_get_contents(&apos;php://stdin&apos;),true);call_user_func_array(&apos;{$function_name}&apos;, \\\$params);\"";<br>          $my_target_exec=strtr(strtr($my_target_exec, $map), $map);<br>          $my_background_exec="(/usr/bin/php -r \"chdir(&apos;{$path_run}&apos;);{$str_requires} my_timeout_exec(\\\"{$my_target_exec}\\\", file_get_contents(&apos;php://stdin&apos;), {$timeout});\" &lt;&amp;3 &amp;) 3&lt;&amp;0";//php by default use "sh", and "sh" don&apos;t support "&lt;&amp;0"<br>          my_timeout_exec($my_background_exec, json_encode($params), 2);<br>         }<br><br>function my_timeout_exec($cmd, $stdin=&apos;&apos;, $timeout)<br>         {$start=time();<br>          $stdout=&apos;&apos;;<br>          $stderr=&apos;&apos;;<br>          //file_put_contents(&apos;debug.txt&apos;, time().&apos;:cmd:&apos;.$cmd."\n", FILE_APPEND);<br>          //file_put_contents(&apos;debug.txt&apos;, time().&apos;:stdin:&apos;.$stdin."\n", FILE_APPEND);<br><br>          $process=proc_open($cmd, [[&apos;pipe&apos;, &apos;r&apos;], [&apos;pipe&apos;, &apos;w&apos;], [&apos;pipe&apos;, &apos;w&apos;]], $pipes);<br>          if (!is_resource($process))<br>             {return array(&apos;return&apos;=&gt;&apos;1&apos;, &apos;stdout&apos;=&gt;$stdout, &apos;stderr&apos;=&gt;$stderr);<br>             }<br>          $status=proc_get_status($process);<br>          posix_setpgid($status[&apos;pid&apos;], $status[&apos;pid&apos;]);    //seperate pgid(process group id) from parent&apos;s pgid<br><br>          stream_set_blocking($pipes[0], 0);<br>          stream_set_blocking($pipes[1], 0);<br>          stream_set_blocking($pipes[2], 0);<br>          fwrite($pipes[0], $stdin);<br>          fclose($pipes[0]);<br><br>          while (1)<br>                {$stdout.=stream_get_contents($pipes[1]);<br>                 $stderr.=stream_get_contents($pipes[2]);<br><br>                 if (time()-$start&gt;$timeout)<br>                    {//proc_terminate($process, 9);    //only terminate subprocess, won&apos;t terminate sub-subprocess<br>                     posix_kill(-$status[&apos;pid&apos;], 9);    //sends SIGKILL to all processes inside group(negative means GPID, all subprocesses share the top process group, except nested my_timeout_exec)<br>                     //file_put_contents(&apos;debug.txt&apos;, time().":kill group {$status[&apos;pid&apos;]}\n", FILE_APPEND);<br>                     return array(&apos;return&apos;=&gt;&apos;1&apos;, &apos;stdout&apos;=&gt;$stdout, &apos;stderr&apos;=&gt;$stderr);<br>                    }<br><br>                 $status=proc_get_status($process);<br>                 //file_put_contents(&apos;debug.txt&apos;, time().&apos;:status:&apos;.var_export($status, true)."\n";<br>                 if (!$status[&apos;running&apos;])<br>                    {fclose($pipes[1]);<br>                     fclose($pipes[2]);<br>                     proc_close($process);<br>                     return $status[&apos;exitcode&apos;];<br>                    }<br><br>                 usleep(100000); <br>                }<br>         }<br>?>
```
<br><br>a_class.php:<br>

```
<?php<br>class A<br>{<br>    static function jack($a, $b)<br>           {sleep(4);<br>            file_put_contents(&apos;debug.txt&apos;, time().":A::jack:".$a.&apos; &apos;.$b."\n", FILE_APPEND);<br>            sleep(15);<br>           }<br>}<br>?>
```
<br><br>test.php:<br>

```
<?php<br>require &apos;my_exec.php&apos;;<br><br>my_background_exec(&apos;A::jack&apos;, array(&apos;hello&apos;, &apos;jack&apos;), &apos;require "my_exec.php";require "a_class.php";&apos;, 8);<br>?>
```
apos;=&gt;&apos;\

```
<?php<br>function my_background_exec($function_name, $params, $str_requires, $timeout=600)<br>         {$map=array(&apos;"&apos;=&gt;&apos;\"&apos;, &apos;$&apos;=&gt;&apos;\$&apos;, &apos;`&apos;=&gt;&apos;\`&apos;, &apos;\\&apos;=&gt;&apos;\\\\&apos;, &apos;!&apos;=&gt;&apos;\!&apos;);<br>          $str_requires=strtr($str_requires, $map);<br>          $path_run=dirname($_SERVER[&apos;SCRIPT_FILENAME&apos;]);<br>          $my_target_exec="/usr/bin/php -r \"chdir(&apos;{$path_run}&apos;);{$str_requires} \\\$params=json_decode(file_get_contents(&apos;php://stdin&apos;),true);call_user_func_array(&apos;{$function_name}&apos;, \\\$params);\"";<br>          $my_target_exec=strtr(strtr($my_target_exec, $map), $map);<br>          $my_background_exec="(/usr/bin/php -r \"chdir(&apos;{$path_run}&apos;);{$str_requires} my_timeout_exec(\\\"{$my_target_exec}\\\", file_get_contents(&apos;php://stdin&apos;), {$timeout});\" &lt;&amp;3 &amp;) 3&lt;&amp;0";//php by default use "sh", and "sh" don&apos;t support "&lt;&amp;0"<br>          my_timeout_exec($my_background_exec, json_encode($params), 2);<br>         }<br><br>function my_timeout_exec($cmd, $stdin=&apos;&apos;, $timeout)<br>         {$start=time();<br>          $stdout=&apos;&apos;;<br>          $stderr=&apos;&apos;;<br>          //file_put_contents(&apos;debug.txt&apos;, time().&apos;:cmd:&apos;.$cmd."\n", FILE_APPEND);<br>          //file_put_contents(&apos;debug.txt&apos;, time().&apos;:stdin:&apos;.$stdin."\n", FILE_APPEND);<br><br>          $process=proc_open($cmd, [[&apos;pipe&apos;, &apos;r&apos;], [&apos;pipe&apos;, &apos;w&apos;], [&apos;pipe&apos;, &apos;w&apos;]], $pipes);<br>          if (!is_resource($process))<br>             {return array(&apos;return&apos;=&gt;&apos;1&apos;, &apos;stdout&apos;=&gt;$stdout, &apos;stderr&apos;=&gt;$stderr);<br>             }<br>          $status=proc_get_status($process);<br>          posix_setpgid($status[&apos;pid&apos;], $status[&apos;pid&apos;]);    //seperate pgid(process group id) from parent&apos;s pgid<br><br>          stream_set_blocking($pipes[0], 0);<br>          stream_set_blocking($pipes[1], 0);<br>          stream_set_blocking($pipes[2], 0);<br>          fwrite($pipes[0], $stdin);<br>          fclose($pipes[0]);<br><br>          while (1)<br>                {$stdout.=stream_get_contents($pipes[1]);<br>                 $stderr.=stream_get_contents($pipes[2]);<br><br>                 if (time()-$start&gt;$timeout)<br>                    {//proc_terminate($process, 9);    //only terminate subprocess, won&apos;t terminate sub-subprocess<br>                     posix_kill(-$status[&apos;pid&apos;], 9);    //sends SIGKILL to all processes inside group(negative means GPID, all subprocesses share the top process group, except nested my_timeout_exec)<br>                     //file_put_contents(&apos;debug.txt&apos;, time().":kill group {$status[&apos;pid&apos;]}\n", FILE_APPEND);<br>                     return array(&apos;return&apos;=&gt;&apos;1&apos;, &apos;stdout&apos;=&gt;$stdout, &apos;stderr&apos;=&gt;$stderr);<br>                    }<br><br>                 $status=proc_get_status($process);<br>                 //file_put_contents(&apos;debug.txt&apos;, time().&apos;:status:&apos;.var_export($status, true)."\n";<br>                 if (!$status[&apos;running&apos;])<br>                    {fclose($pipes[1]);<br>                     fclose($pipes[2]);<br>                     proc_close($process);<br>                     return $status[&apos;exitcode&apos;];<br>                    }<br><br>                 usleep(100000); <br>                }<br>         }<br>?>
```
<br><br>a_class.php:<br>

```
<?php<br>class A<br>{<br>    static function jack($a, $b)<br>           {sleep(4);<br>            file_put_contents(&apos;debug.txt&apos;, time().":A::jack:".$a.&apos; &apos;.$b."\n", FILE_APPEND);<br>            sleep(15);<br>           }<br>}<br>?>
```
<br><br>test.php:<br>

```
<?php<br>require &apos;my_exec.php&apos;;<br><br>my_background_exec(&apos;A::jack&apos;, array(&apos;hello&apos;, &apos;jack&apos;), &apos;require "my_exec.php";require "a_class.php";&apos;, 8);<br>?>
```
apos;, &apos;`&apos;=&gt;&apos;\`&apos;, &apos;\\&apos;=&gt;&apos;\\\\&apos;, &apos;!&apos;=&gt;&apos;\!&apos;);
          $str_requires=strtr($str_requires, $map);
          $path_run=dirname($_SERVER[&apos;SCRIPT_FILENAME&apos;]);
          $my_target_exec="/usr/bin/php -r \"chdir(&apos;{$path_run}&apos;);{$str_requires} \\\$params=json_decode(file_get_contents(&apos;php://stdin&apos;),true);call_user_func_array(&apos;{$function_name}&apos;, \\\$params);\"";
          $my_target_exec=strtr(strtr($my_target_exec, $map), $map);
          $my_background_exec="(/usr/bin/php -r \"chdir(&apos;{$path_run}&apos;);{$str_requires} my_timeout_exec(\\\"{$my_target_exec}\\\", file_get_contents(&apos;php://stdin&apos;), {$timeout});\" &lt;&amp;3 &amp;) 3&lt;&amp;0";//php by default use "sh", and "sh" don&apos;t support "&lt;&amp;0"
          my_timeout_exec($my_background_exec, json_encode($params), 2);
         }

function my_timeout_exec($cmd, $stdin=&apos;&apos;, $timeout)
         {$start=time();
          $stdout=&apos;&apos;;
          $stderr=&apos;&apos;;
          //file_put_contents(&apos;debug.txt&apos;, time().&apos;:cmd:&apos;.$cmd."\n", FILE_APPEND);
          //file_put_contents(&apos;debug.txt&apos;, time().&apos;:stdin:&apos;.$stdin."\n", FILE_APPEND);

          $process=proc_open($cmd, [[&apos;pipe&apos;, &apos;r&apos;], [&apos;pipe&apos;, &apos;w&apos;], [&apos;pipe&apos;, &apos;w&apos;]], $pipes);
          if (!is_resource($process))
             {return array(&apos;return&apos;=&gt;&apos;1&apos;, &apos;stdout&apos;=&gt;$stdout, &apos;stderr&apos;=&gt;$stderr);
             }
          $status=proc_get_status($process);
          posix_setpgid($status[&apos;pid&apos;], $status[&apos;pid&apos;]);    //seperate pgid(process group id) from parent&apos;s pgid

          stream_set_blocking($pipes[0], 0);
          stream_set_blocking($pipes[1], 0);
          stream_set_blocking($pipes[2], 0);
          fwrite($pipes[0], $stdin);
          fclose($pipes[0]);

          while (1)
                {$stdout.=stream_get_contents($pipes[1]);
                 $stderr.=stream_get_contents($pipes[2]);

                 if (time()-$start&gt;$timeout)
                    {//proc_terminate($process, 9);    //only terminate subprocess, won&apos;t terminate sub-subprocess
                     posix_kill(-$status[&apos;pid&apos;], 9);    //sends SIGKILL to all processes inside group(negative means GPID, all subprocesses share the top process group, except nested my_timeout_exec)
                     //file_put_contents(&apos;debug.txt&apos;, time().":kill group {$status[&apos;pid&apos;]}\n", FILE_APPEND);
                     return array(&apos;return&apos;=&gt;&apos;1&apos;, &apos;stdout&apos;=&gt;$stdout, &apos;stderr&apos;=&gt;$stderr);
                    }

                 $status=proc_get_status($process);
                 //file_put_contents(&apos;debug.txt&apos;, time().&apos;:status:&apos;.var_export($status, true)."\n";
                 if (!$status[&apos;running&apos;])
                    {fclose($pipes[1]);
                     fclose($pipes[2]);
                     proc_close($process);
                     return $status[&apos;exitcode&apos;];
                    }

                 usleep(100000); 
                }
         }
?>
```


a_class.php:


```
<?php
class A
{
    static function jack($a, $b)
           {sleep(4);
            file_put_contents(&apos;debug.txt&apos;, time().":A::jack:".$a.&apos; &apos;.$b."\n", FILE_APPEND);
            sleep(15);
           }
}
?>
```


test.php:


```
<?php
require &apos;my_exec.php&apos;;

my_background_exec(&apos;A::jack&apos;, array(&apos;hello&apos;, &apos;jack&apos;), &apos;require "my_exec.php";require "a_class.php";&apos;, 8);
?>
```
  

#

To get the currently used time, use getrusage()  

#

I was having trouble with script timeouts in applications where the user prompted long running background actions. I wrote this cURL/CLI background script that solved the problem when making requests from HTTP.<br><br>

```
<?php

/* BACKGROUND CLI 1.0
   
   eric pecoraro _at_ shepard dot com - 2005-06-02
   Use at your own risk. No warranties expressed or implied.

   Include this file at the top of any script to run it in the background
   with no time limitations ... e.g., include(&apos;background_cli.php&apos;);
   
   The script that calls this file should not return output to the browser. 
*/
#  REQUIREMENTS - cURL and CLI
   if ( !function_exists(&apos;curl_setopt&apos;) OR !function_exists(&apos;curl_setopt&apos;)  ) {
      echo &apos;Requires cURL and CLI installations.&apos; ; exit ; 
   }
   
#  BUILD PATHS
   $script = array_pop(explode(&apos;/&apos;,$SCRIPT_NAME)) ; 
   $script_dir = substr($SCRIPT_NAME,0,strlen($SCRIPT_NAME)-strlen($script)) ;
   $scriptURL = &apos;http://&apos;. $HTTP_HOST . $script_dir . "$script" ;
   $curlURL = &apos;http://&apos;. $HTTP_HOST . $script_dir . "$script?runscript=curl" ;

#  Indicate that script is being called by CLI 
   if ( php_sapi_name() == &apos;cli&apos; ) {
      $CLI = true ;
   }

#  Action if script is being called by cURL_prompt()
   if ( $runscript == &apos;curl&apos; ) {
      $cmd = "/usr/local/bin/php ".$PATH_TRANSLATED ; // server location of script to run
      exec($cmd) ;
      exit;
   }

#  USER INTERFACE
   // User answer after submission.
   if ( $post ) {
      cURL_prompt($curlURL) ;
      echo &apos;&lt;div style="margin:25px;"&gt;&lt;title&gt;Background CLI&lt;/title&gt;&apos;;
      echo &apos;O.K. If all goes well, &lt;b&gt;&apos;.$script.&apos;&lt;/b&gt; is working hard in the background with no &apos; ;
      echo &apos;timeout limitations. &lt;br&gt;&lt;br&gt;&lt;form action=&apos;.$scriptURL.&apos; method=GET&gt;&apos; ;
      echo &apos;&lt;input type=submit value=" RESET BACKGROUND CLI "&gt;&lt;/form&gt;&lt;/div&gt;&apos; ;
      exit ;
   }
   // Start screen.
   if ( !$CLI AND !$runscript ) {
      echo &apos;&lt;title&gt;Background CLI&lt;/title&gt;&lt;div style="margin:25px;"&gt;&apos; ;
      echo &apos;&lt;form action=&apos;.$scriptURL.&apos; method=POST&gt;&apos; ;
      echo &apos;Click to run &lt;b&gt;&apos;.$script.&apos;&lt;/b&gt; from the PHP CLI command line, in the background.&lt;br&gt;&lt;br&gt;&apos; ;
      echo &apos;&lt;input type=hidden value=1 name=post&gt;&apos; ;
      echo &apos;&lt;input type=submit value=" RUN IN BACKGROUND "&gt;&lt;/form&gt;&lt;/div&gt;&apos; ;
      exit ;
   }

#  cURL URL PROMPT FUNCTION
   function cURL_prompt($url_path) {
      ob_start(); // start output buffer
      $c=curl_init($url_path);
      curl_setopt($c, CURLOPT_TIMEOUT, 2); // drop connection after 2 seconds
      curl_exec($c);
      curl_close($c);
      ob_end_clean(); // discard output buffer
   }
?>
```
  

#

In IIS, there is another global timeout setting which will override any PHP settings.  You can alter this timeout by following the following instructions:<br><br>http://www.iisadmin.co.uk/?p=7  

#

[Official documentation page](https://www.php.net/manual/en/function.set-time-limit.php)

**[To root](/README.md)**