# shell_exec



If you&apos;re trying to run a command such as "gunzip -t" in shell_exec and getting an empty result, you might need to add 2&gt;&amp;1 to the end of the command, eg:<br><br>Won&apos;t always work:<br>echo shell_exec("gunzip -c -t $path_to_backup_file");<br><br>Should work:<br>echo shell_exec("gunzip -c -t $path_to_backup_file 2&gt;&amp;1");<br><br>In the above example, a line break at the beginning of the gunzip output seemed to prevent shell_exec printing anything else. Hope this saves someone else an hour or two.  

#

To run a command in background, the output must be redirected to /dev/null. This is written in exec() manual page. There are cases where you need the output to be logged somewhere else though. Redirecting the output to a file like this didn&apos;t work for me:<br><br>

```
<?php
# this doesn't work!
shell_exec("my_script.sh 2&gt;&amp;1 &gt;&gt; /tmp/mylog &amp;");
?>
```


Using the above command still hangs web browser request.

Seems like you have to add exactly "/dev/null" to the command line. For instance, this worked:



```
<?php
# works, but output is lost
shell_exec("my_script.sh 2&gt;/dev/null &gt;/dev/null &amp;");
?>
```


But I wanted the output, so I used this:



```
<?php
shell_exec("my_script.sh 2&gt;&amp;1 | tee -a /tmp/mylog 2&gt;/dev/null &gt;/dev/null &amp;");
?>
```
<br><br>Hope this helps someone.  

#

A simple way to handle the problem of capturing stderr output when using shell-exec under windows is to call ob_start() before the command and ob_end_clean() afterwards, like this:<br><br>

```
<?php
ob_start()
$dir = shell_exec('dir B:');
if is_null($dir)
{   // B: does not exist
    // do whatever you want with the stderr output here
}
else
{  // B: exists and $dir holds the directory listing
   // do whatever you want with it here
}
ob_end_clean();   // get rid of the evidence :-)
?>
```
<br><br>If B: does not exist then $dir will be Null and the output buffer will have captured the message: <br><br>  &apos;The system cannot find the path specified&apos;. <br><br>(under WinXP, at least). If B: exists then $dir will contain the directory listing and we probably don&apos;t care about the output buffer. In any case it needs to be deleted before proceeding.  

#

[Official documentation page](https://www.php.net/manual/en/function.shell-exec.php)

**[To root](/README.md)**