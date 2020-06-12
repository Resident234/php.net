# syslog




<div class="phpcode"><span class="html">
A word of warning; if you use openlog() to ready syslog() and your Apache threads accept multiple requests, you *must* call closelog() if Apache&apos;s error log is configured to write to syslog.&#xA0; Failure to do so will cause Apache&apos;s error log to write to whatever facility/ident was used in openlog.<br><br>Example, in httpd.conf you have:<br><br>ErrorLog syslog:local7<br><br>and in php you do:<br><br><span class="default">&lt;?php<br>openlog</span><span class="keyword">(</span><span class="string">&quot;myprogram&quot;</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, </span><span class="default">LOG_LOCAL0</span><span class="keyword">);<br></span><span class="default">syslog</span><span class="keyword">(</span><span class="string">&quot;My syslog message&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>From here on out, this Apache thread will write ErrorLog to local0 and under the process name &quot;myprogram&quot; and not httpd!&#xA0; Calling closelog() will fix this.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.syslog.php)

**[⬆ to root](/)**