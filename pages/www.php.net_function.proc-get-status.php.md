# proc_get_status




<div class="phpcode"><span class="html">
On Unix/Linux, if you change the command line you pass to proc_open() just slightly then proc_get_status() will give you the actual process-id (pid) of your child.<br><br>Suppose you wish to run the external command /usr/bin/compress to create a BSD foo.Z file.&#xA0; Rather than proc_open(&quot;/usr/bin/compress /tmp/foo&quot;,...) you may invoke proc_open(&quot;exec /usr/bin/compress /tmp/foo&quot;,...) and then proc_get_status()[&apos;pid&apos;] will be the actual pid of /usr/bin/compress.<br><br>Why?&#xA0; Because the way proc_open() actually works on Unix/Linux is by starting &quot;/bin/sh -c usercmd userargs...&quot;, e.g., &quot;/bin/sh -c /usr/bin/compress /tmp/foo&quot;.[Note 1]&#xA0; That means normally your command is the child of the shell, so the pid you retrieve with proc_get_status() is the pid of the shell (PHP&apos;s child), and you have to fumble around trying to find the pid of your command (PHP&apos;s grandchild).&#xA0; But if you put &quot;exec&quot; in front of your command, you tell the shell to *replace itself* with your command without starting another process (technically, to exec your command without forking first).&#xA0; That means your command will inherit the pid of the shell, which is the pid that proc_get_status() returns.<br><br>So if you would like the actual pid of the process running your command, just prepend &quot;exec &quot; to&#xA0; your proc_open() command argument then retrieve the pid using proc_get_status().<br><br>This also makes proc_terminate() and proc_close() work more like you might prefer, since they will affect the actual process running your command (which will be a child process rather than a grandchild process).<br><br>[Note 1] My guess is that the PHP developers want the shell to expand wildcards in path/filenames.</span>
</div>
  

#


<div class="phpcode"><span class="html">
It is worth noting that proc_get_status will continue to indicate the process that you spawned is running (because it is!) until that process has been able to write everything it wants to write to the STDOUT and STDERR streams.<br><br>PHP seems to use a buffer for this and so the spawned process can can get it&apos;s write calls to return immediately. <br><br>However, once this buffer is full the write call will block until you read out some of the information from the stream/pipe.<br><br>This can manifest itself in many ways but generally the called process will still be running, but just not doing anything as it is blocking on being able to write more to STDERR or STDOUT -- whichever stream buffer is full.<br><br>To work around this you should include in your loop of checking proc_get_status&apos; running element a &quot;stream_get_contents&quot; on the relevant pipes.<br><br>I generally use stream_set_blocking($pipies[2], 0) kind of calls to make sure that the stream_get_contents call will not block if there is no data in the stream.<br><br>This one had me stumped for a while, so hopefully it helps someone!</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.proc-get-status.php)

**[To root](/README.md)**