# syslog



A word of warning; if you use openlog() to ready syslog() and your Apache threads accept multiple requests, you *must* call closelog() if Apache&apos;s error log is configured to write to syslog.  Failure to do so will cause Apache&apos;s error log to write to whatever facility/ident was used in openlog.<br><br>Example, in httpd.conf you have:<br><br>ErrorLog syslog:local7<br><br>and in php you do:<br><br>

```
<?php
openlog("myprogram", 0, LOG_LOCAL0);
syslog("My syslog message");
?>
```
<br><br>From here on out, this Apache thread will write ErrorLog to local0 and under the process name "myprogram" and not httpd!  Calling closelog() will fix this.  

---

[Official documentation page](https://www.php.net/manual/en/function.syslog.php)

**[To root](/README.md)**