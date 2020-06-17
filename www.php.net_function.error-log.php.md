# error_log




<div class="phpcode"><span class="html">
Advice to novices: This function works great along with &quot;tail&quot; which is a unix command to watch a log file live. There are versions of Tail for Windows too, like Tail for Win32 or Kiwi Log Viewer.<br><br>Using both error_log() and tail to view the php_error.log you can debug code without having to worry so much about printing debug messages to the screen and who they might be seen by.<br><br>Further Note: This works even better when you have two monitors setup. One for your browser and IDE and the other for viewing the log files update live as you go.</span>
</div>
  

#


<div class="phpcode"><span class="html">
DO NOT try to output TOO LARGE texts in the error_log();<br><br>if you try to output massive amounts of texts it will either cut of the text at about 8ooo characters (for reasonable massive strings, &lt; 32 K characters) or (for insanely massive strings, about 1.6 million characters) totally crash without even throwing an error or anything (I even put it in a try/catch without getting any result from the catch).<br><br>I had this problem when I tried to debug a response from a wp_remote_get(); all of my error_log() worked as they should, except for ONE of them... (-_-)<br>After about a day of debugging I finally found out why &amp; that&apos;s why I type this.<br><br>Apparently the response contained a body with over 1.6 million chars (or bytes? (whatever strlen() returns)).<br><br>If you have a string of unknown length, use this:<br>$start_index = 0;<br>$end_index = 8000;<br>error_log( substr( $output_text , $start_index , $end_index ) );</span>
</div>
  

#


<div class="phpcode"><span class="html">
There is a limit on the maximum length that you can pass as the $message.<br><br>The default seem to be 1024 but can be changed by adjusting the value of the runtime configuration value of &apos;log_errors_max_len&apos;.<br><br>More details here:<br><a href="http://www.php.net/manual/en/errorfunc.configuration.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/errorfunc.configuration.php</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
Beware!&#xA0; If multiple scripts share the same log file, but run as different users, whichever script logs an error first owns the file, and calls to error_log() run as a different user will fail *silently*!<br><br>Nothing more frustrating than trying to figure out why all your error_log calls aren&apos;t actually writing, than to find it was due to a *silent* permission denied error!</span>
</div>
  

#


<div class="phpcode"><span class="html">
when using error_log to send email, not all elements of an extra_headers string are handled the same way.&#xA0; &quot;From: &quot; and &quot;Reply-To: &quot; header values will replace the default header values. &quot;Subject: &quot; header values won&apos;t: they are *added* to the mail header but don&apos;t replace the default, leading to mail messages with two Subject fields.<br><br><span class="default">&lt;?php<br><br>error_log</span><span class="keyword">(</span><span class="string">&quot;sometext&quot;</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">, </span><span class="string">&quot;zigzag@my.domain&quot;</span><span class="keyword">, <br>&#xA0; </span><span class="string">&quot;Subject: Foo\nFrom: Rizzlas@my.domain\n&quot;</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>---------------%&lt;-----------------------<br>To: zigzag@my.domain<br>Envelope-to: zigzag@my.domain<br>Date: Fri, 28 Mar 2003 13:29:02 -0500<br>From: Rizzlas@my.domain<br>Subject: PHP error_log message<br>Subject: Foo<br>Delivery-date: Fri, 28 Mar 2003 13:29:03 -0500<br><br>sometext<br>---------------%&lt;---------------------<br><br>quoth the docs: &quot;This message type uses the same internal function as mail() does.&quot;&#xA0; <br><br>mail() will also fail to set a Subject field based on extra_header data - instead it takes a seperate argument to specify a &quot;Subject: &quot; string.<br><br>php v.4.2.3, SunOS 5.8</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.error-log.php)

**[To root](/README.md)**