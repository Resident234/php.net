# socket_read




<div class="phpcode"><span class="html">
quote:<br>&quot;Note:<br>socket_read() returns a zero length string (&quot;&quot;) when there is no more data to read.&quot;<br><br>This is not true!<br><br>In a while loop&#xA0; <br>(example case few bytes to receive - just enough for 1 call, but you use a loop to be sure you received all data)<br>if you use <br>&lt;? socket_set_block($socket); ?&gt;<br>you will get:<br>1st call in loop: data<br>2nd call in loop: a block forever, if there isnt data anymore or w/e happen to the &quot;other side&quot;<br><br>So ofc you want to use <br>&lt;? socket_set_nonblock($socket); ?&gt;<br>and you will get:<br>1st call in loop: data<br>2nd call in loop: socket_read() returns FALSE (bool) and socket_last_error() gives you a SOCKET_EWOULDBLOCK (<a href="http://de1.php.net/manual/de/sockets.constants.php" rel="nofollow" target="_blank">http://de1.php.net/manual/de/sockets.constants.php</a>)<br><br>There is not a single time i got a empty string back from socket_read.<br>And im &quot;working&quot; on this problem(bug?) since a week or so.<br><br>You better use socket_recv() instead.<br>(good luck)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.socket-read.php)

**[⬆ to root](/)**