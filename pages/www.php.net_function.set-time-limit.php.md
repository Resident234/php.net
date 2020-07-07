# set_time_limit





while setting the set_time_limit(), the duration of sleep() will be ignored in the execution time. The following illustrates:



```
<?php

set_time_limit(20);

while ($i&lt;=10)
{
&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;i=$i &quot;;
&#xA0; &#xA0; &#xA0; &#xA0; sleep(100);
&#xA0; &#xA0; &#xA0; &#xA0; $i++;
}

php?>
```


Output:
i=0 i=1 i=2 i=3 i=4 i=5 i=6 i=7 i=8 i=9 i=10

  

#



Both set_time_limit(...) and&#xA0; ini_set(&apos;max_execution_time&apos;,...); won&apos;t count the time cost of sleep,file_get_contents,shell_exec,mysql_query etc, so i build this function my_background_exec(), to run static method/function in background/detached process and time is out kill it:

my_exec.php:


```
<?php
function my_background_exec($function_name, $params, $str_requires, $timeout=600)
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {$map=array(&apos;&quot;&apos;=&gt;&apos;\&quot;&apos;, &apos;$&apos;=&gt;&apos;\$&apos;, &apos;`&apos;=&gt;&apos;\`&apos;, &apos;\\&apos;=&gt;&apos;\\\\&apos;, &apos;!&apos;=&gt;&apos;\!&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $str_requires=strtr($str_requires, $map);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $path_run=dirname($_SERVER[&apos;SCRIPT_FILENAME&apos;]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $my_target_exec=&quot;/usr/bin/php -r \&quot;chdir(&apos;{$path_run}&apos;);{$str_requires} \\\$params=json_decode(file_get_contents(&apos;php://stdin&apos;),true);call_user_func_array(&apos;{$function_name}&apos;, \\\$params);\&quot;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $my_target_exec=strtr(strtr($my_target_exec, $map), $map);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $my_background_exec=&quot;(/usr/bin/php -r \&quot;chdir(&apos;{$path_run}&apos;);{$str_requires} my_timeout_exec(\\\&quot;{$my_target_exec}\\\&quot;, file_get_contents(&apos;php://stdin&apos;), {$timeout});\&quot; &lt;&amp;3 &amp;) 3&lt;&amp;0&quot;;//php by default use &quot;sh&quot;, and &quot;sh&quot; don&apos;t support &quot;&lt;&amp;0&quot;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; my_timeout_exec($my_background_exec, json_encode($params), 2);
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }

function my_timeout_exec($cmd, $stdin=&apos;&apos;, $timeout)
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {$start=time();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $stdout=&apos;&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $stderr=&apos;&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //file_put_contents(&apos;debug.txt&apos;, time().&apos;:cmd:&apos;.$cmd.&quot;\n&quot;, FILE_APPEND);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //file_put_contents(&apos;debug.txt&apos;, time().&apos;:stdin:&apos;.$stdin.&quot;\n&quot;, FILE_APPEND);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $process=proc_open($cmd, [[&apos;pipe&apos;, &apos;r&apos;], [&apos;pipe&apos;, &apos;w&apos;], [&apos;pipe&apos;, &apos;w&apos;]], $pipes);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!is_resource($process))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {return array(&apos;return&apos;=&gt;&apos;1&apos;, &apos;stdout&apos;=&gt;$stdout, &apos;stderr&apos;=&gt;$stderr);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $status=proc_get_status($process);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; posix_setpgid($status[&apos;pid&apos;], $status[&apos;pid&apos;]);&#xA0; &#xA0; //seperate pgid(process group id) from parent&apos;s pgid

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; stream_set_blocking($pipes[0], 0);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; stream_set_blocking($pipes[1], 0);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; stream_set_blocking($pipes[2], 0);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fwrite($pipes[0], $stdin);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fclose($pipes[0]);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; while (1)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {$stdout.=stream_get_contents($pipes[1]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $stderr.=stream_get_contents($pipes[2]);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; if (time()-$start&gt;$timeout)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {//proc_terminate($process, 9);&#xA0; &#xA0; //only terminate subprocess, won&apos;t terminate sub-subprocess
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; posix_kill(-$status[&apos;pid&apos;], 9);&#xA0; &#xA0; //sends SIGKILL to all processes inside group(negative means GPID, all subprocesses share the top process group, except nested my_timeout_exec)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; //file_put_contents(&apos;debug.txt&apos;, time().&quot;:kill group {$status[&apos;pid&apos;]}\n&quot;, FILE_APPEND);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return array(&apos;return&apos;=&gt;&apos;1&apos;, &apos;stdout&apos;=&gt;$stdout, &apos;stderr&apos;=&gt;$stderr);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $status=proc_get_status($process);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; //file_put_contents(&apos;debug.txt&apos;, time().&apos;:status:&apos;.var_export($status, true).&quot;\n&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; if (!$status[&apos;running&apos;])
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {fclose($pipes[1]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; fclose($pipes[2]);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; proc_close($process);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return $status[&apos;exitcode&apos;];
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; usleep(100000); 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
php?>
```


a_class.php:


```
<?php
class A
{
&#xA0; &#xA0; static function jack($a, $b)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {sleep(4);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; file_put_contents(&apos;debug.txt&apos;, time().&quot;:A::jack:&quot;.$a.&apos; &apos;.$b.&quot;\n&quot;, FILE_APPEND);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; sleep(15);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
}
php?>
```


test.php:


```
<?php
require &apos;my_exec.php&apos;;

my_background_exec(&apos;A::jack&apos;, array(&apos;hello&apos;, &apos;jack&apos;), &apos;require &quot;my_exec.php&quot;;require &quot;a_class.php&quot;;&apos;, 8);
php?>
```



  

#



To get the currently used time, use getrusage()

  

#



I was having trouble with script timeouts in applications where the user prompted long running background actions. I wrote this cURL/CLI background script that solved the problem when making requests from HTTP.



```
<?php

/* BACKGROUND CLI 1.0
&#xA0;&#xA0; 
&#xA0;&#xA0; eric pecoraro _at_ shepard dot com - 2005-06-02
&#xA0;&#xA0; Use at your own risk. No warranties expressed or implied.

&#xA0;&#xA0; Include this file at the top of any script to run it in the background
&#xA0;&#xA0; with no time limitations ... e.g., include(&apos;background_cli.php&apos;);
&#xA0;&#xA0; 
&#xA0;&#xA0; The script that calls this file should not return output to the browser. 
*/
#&#xA0; REQUIREMENTS - cURL and CLI
&#xA0;&#xA0; if ( !function_exists(&apos;curl_setopt&apos;) OR !function_exists(&apos;curl_setopt&apos;)&#xA0; ) {
&#xA0; &#xA0; &#xA0; echo &apos;Requires cURL and CLI installations.&apos; ; exit ; 
&#xA0;&#xA0; }
&#xA0;&#xA0; 
#&#xA0; BUILD PATHS
&#xA0;&#xA0; $script = array_pop(explode(&apos;/&apos;,$SCRIPT_NAME)) ; 
&#xA0;&#xA0; $script_dir = substr($SCRIPT_NAME,0,strlen($SCRIPT_NAME)-strlen($script)) ;
&#xA0;&#xA0; $scriptURL = &apos;http://&apos;. $HTTP_HOST . $script_dir . &quot;$script&quot; ;
&#xA0;&#xA0; $curlURL = &apos;http://&apos;. $HTTP_HOST . $script_dir . &quot;$script?runscript=curl&quot; ;

#&#xA0; Indicate that script is being called by CLI 
&#xA0;&#xA0; if ( php_sapi_name() == &apos;cli&apos; ) {
&#xA0; &#xA0; &#xA0; $CLI = true ;
&#xA0;&#xA0; }

#&#xA0; Action if script is being called by cURL_prompt()
&#xA0;&#xA0; if ( $runscript == &apos;curl&apos; ) {
&#xA0; &#xA0; &#xA0; $cmd = &quot;/usr/local/bin/php &quot;.$PATH_TRANSLATED ; // server location of script to run
&#xA0; &#xA0; &#xA0; exec($cmd) ;
&#xA0; &#xA0; &#xA0; exit;
&#xA0;&#xA0; }

#&#xA0; USER INTERFACE
&#xA0;&#xA0; // User answer after submission.
&#xA0;&#xA0; if ( $post ) {
&#xA0; &#xA0; &#xA0; cURL_prompt($curlURL) ;
&#xA0; &#xA0; &#xA0; echo &apos;&lt;div style=&quot;margin:25px;&quot;&gt;&lt;title&gt;Background CLI&lt;/title&gt;&apos;;
&#xA0; &#xA0; &#xA0; echo &apos;O.K. If all goes well, &lt;b&gt;&apos;.$script.&apos;&lt;/b&gt; is working hard in the background with no &apos; ;
&#xA0; &#xA0; &#xA0; echo &apos;timeout limitations. &lt;br&gt;&lt;br&gt;&lt;form action=&apos;.$scriptURL.&apos; method=GET&gt;&apos; ;
&#xA0; &#xA0; &#xA0; echo &apos;&lt;input type=submit value=&quot; RESET BACKGROUND CLI &quot;&gt;&lt;/form&gt;&lt;/div&gt;&apos; ;
&#xA0; &#xA0; &#xA0; exit ;
&#xA0;&#xA0; }
&#xA0;&#xA0; // Start screen.
&#xA0;&#xA0; if ( !$CLI AND !$runscript ) {
&#xA0; &#xA0; &#xA0; echo &apos;&lt;title&gt;Background CLI&lt;/title&gt;&lt;div style=&quot;margin:25px;&quot;&gt;&apos; ;
&#xA0; &#xA0; &#xA0; echo &apos;&lt;form action=&apos;.$scriptURL.&apos; method=POST&gt;&apos; ;
&#xA0; &#xA0; &#xA0; echo &apos;Click to run &lt;b&gt;&apos;.$script.&apos;&lt;/b&gt; from the PHP CLI command line, in the background.&lt;br&gt;&lt;br&gt;&apos; ;
&#xA0; &#xA0; &#xA0; echo &apos;&lt;input type=hidden value=1 name=post&gt;&apos; ;
&#xA0; &#xA0; &#xA0; echo &apos;&lt;input type=submit value=&quot; RUN IN BACKGROUND &quot;&gt;&lt;/form&gt;&lt;/div&gt;&apos; ;
&#xA0; &#xA0; &#xA0; exit ;
&#xA0;&#xA0; }

#&#xA0; cURL URL PROMPT FUNCTION
&#xA0;&#xA0; function cURL_prompt($url_path) {
&#xA0; &#xA0; &#xA0; ob_start(); // start output buffer
&#xA0; &#xA0; &#xA0; $c=curl_init($url_path);
&#xA0; &#xA0; &#xA0; curl_setopt($c, CURLOPT_TIMEOUT, 2); // drop connection after 2 seconds
&#xA0; &#xA0; &#xA0; curl_exec($c);
&#xA0; &#xA0; &#xA0; curl_close($c);
&#xA0; &#xA0; &#xA0; ob_end_clean(); // discard output buffer
&#xA0;&#xA0; }
php?>
```



  

#



In IIS, there is another global timeout setting which will override any PHP settings.&#xA0; You can alter this timeout by following the following instructions:

http://www.iisadmin.co.uk/?p=7

  

#

[Official documentation page](https://www.php.net/manual/en/function.set-time-limit.php)

**[To root](/README.md)**