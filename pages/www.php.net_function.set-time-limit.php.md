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
         {$map=array('"'=>'\"', '  =>'\  , '`'=>'\`', '\\'=>'\\\\', '!'=>'\!');
          $str_requires=strtr($str_requires, $map);
          $path_run=dirname($_SERVER['SCRIPT_FILENAME']);
          $my_target_exec="/usr/bin/php -r \"chdir('{$path_run}');{$str_requires} \\\$params=json_decode(file_get_contents('php://stdin'),true);call_user_func_array('{$function_name}', \\\$params);\"";
          $my_target_exec=strtr(strtr($my_target_exec, $map), $map);
          $my_background_exec="(/usr/bin/php -r \"chdir('{$path_run}');{$str_requires} my_timeout_exec(\\\"{$my_target_exec}\\\", file_get_contents('php://stdin'), {$timeout});\" &lt;&amp;3 &amp;) 3&lt;&amp;0";//php by default use "sh", and "sh" don't support "&lt;&amp;0"
          my_timeout_exec($my_background_exec, json_encode($params), 2);
         }

function my_timeout_exec($cmd, $stdin='', $timeout)
         {$start=time();
          $stdout='';
          $stderr='';
          //file_put_contents('debug.txt', time().':cmd:'.$cmd."\n", FILE_APPEND);
          //file_put_contents('debug.txt', time().':stdin:'.$stdin."\n", FILE_APPEND);

          $process=proc_open($cmd, [['pipe', 'r'], ['pipe', 'w'], ['pipe', 'w']], $pipes);
          if (!is_resource($process))
             {return array('return'=>'1', 'stdout'=>$stdout, 'stderr'=>$stderr);
             }
          $status=proc_get_status($process);
          posix_setpgid($status['pid'], $status['pid']);    //seperate pgid(process group id) from parent's pgid

          stream_set_blocking($pipes[0], 0);
          stream_set_blocking($pipes[1], 0);
          stream_set_blocking($pipes[2], 0);
          fwrite($pipes[0], $stdin);
          fclose($pipes[0]);

          while (1)
                {$stdout.=stream_get_contents($pipes[1]);
                 $stderr.=stream_get_contents($pipes[2]);

                 if (time()-$start&gt;$timeout)
                    {//proc_terminate($process, 9);    //only terminate subprocess, won't terminate sub-subprocess
                     posix_kill(-$status['pid'], 9);    //sends SIGKILL to all processes inside group(negative means GPID, all subprocesses share the top process group, except nested my_timeout_exec)
                     //file_put_contents('debug.txt', time().":kill group {$status['pid']}\n", FILE_APPEND);
                     return array('return'=>'1', 'stdout'=>$stdout, 'stderr'=>$stderr);
                    }

                 $status=proc_get_status($process);
                 //file_put_contents('debug.txt', time().':status:'.var_export($status, true)."\n";
                 if (!$status['running'])
                    {fclose($pipes[1]);
                     fclose($pipes[2]);
                     proc_close($process);
                     return $status['exitcode'];
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
            file_put_contents('debug.txt', time().":A::jack:".$a.' '.$b."\n", FILE_APPEND);
            sleep(15);
           }
}
?>
```


test.php:


```
<?php
require 'my_exec.php';

my_background_exec('A::jack', array('hello', 'jack'), 'require "my_exec.php";require "a_class.php";', 8);
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
   with no time limitations ... e.g., include('background_cli.php');
   
   The script that calls this file should not return output to the browser. 
*/
#  REQUIREMENTS - cURL and CLI
   if ( !function_exists('curl_setopt') OR !function_exists('curl_setopt')  ) {
      echo 'Requires cURL and CLI installations.' ; exit ; 
   }
   
#  BUILD PATHS
   $script = array_pop(explode('/',$SCRIPT_NAME)) ; 
   $script_dir = substr($SCRIPT_NAME,0,strlen($SCRIPT_NAME)-strlen($script)) ;
   $scriptURL = 'http://'. $HTTP_HOST . $script_dir . "$script" ;
   $curlURL = 'http://'. $HTTP_HOST . $script_dir . "$script?runscript=curl" ;

#  Indicate that script is being called by CLI 
   if ( php_sapi_name() == 'cli' ) {
      $CLI = true ;
   }

#  Action if script is being called by cURL_prompt()
   if ( $runscript == 'curl' ) {
      $cmd = "/usr/local/bin/php ".$PATH_TRANSLATED ; // server location of script to run
      exec($cmd) ;
      exit;
   }

#  USER INTERFACE
   // User answer after submission.
   if ( $post ) {
      cURL_prompt($curlURL) ;
      echo '&lt;div style="margin:25px;"&gt;&lt;title&gt;Background CLI&lt;/title&gt;';
      echo 'O.K. If all goes well, &lt;b&gt;'.$script.'&lt;/b&gt; is working hard in the background with no ' ;
      echo 'timeout limitations. &lt;br&gt;&lt;br&gt;&lt;form action='.$scriptURL.' method=GET&gt;' ;
      echo '&lt;input type=submit value=" RESET BACKGROUND CLI "&gt;&lt;/form&gt;&lt;/div&gt;' ;
      exit ;
   }
   // Start screen.
   if ( !$CLI AND !$runscript ) {
      echo '&lt;title&gt;Background CLI&lt;/title&gt;&lt;div style="margin:25px;"&gt;' ;
      echo '&lt;form action='.$scriptURL.' method=POST&gt;' ;
      echo 'Click to run &lt;b&gt;'.$script.'&lt;/b&gt; from the PHP CLI command line, in the background.&lt;br&gt;&lt;br&gt;' ;
      echo '&lt;input type=hidden value=1 name=post&gt;' ;
      echo '&lt;input type=submit value=" RUN IN BACKGROUND "&gt;&lt;/form&gt;&lt;/div&gt;' ;
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