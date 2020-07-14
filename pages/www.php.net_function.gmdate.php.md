# gmdate



If you have the same application running in different countries, you may have some troubles getting the local time..<br>In my case, I was having troubles with a clock created with Macromedia Flash... the time shown by the clock was supposed to be set up by the server, passing the timestamp. When I moved the file to another country, I got a wrong time...<br>You can use the timezone offset ( date("Z") ) to handle this kind of thing...<br><br>

```
<?php
$timestamp = time()+date("Z");
echo gmdate("Y/m/d H:i:s",$timestamp);
?>
```
  

#

Wath out for summer time and winter time...<br><br>If you want to get the current date and time based on GMT you could use this :<br><br>

```
<?php
$timezone  = -5; //(GMT -5:00) EST (U.S. &amp; Canada)
echo gmdate("Y/m/j H:i:s", time() + 3600*($timezone+date("I"))); 
?>
```
<br><br>this would gives: 2004/07/8 14:35:19 in summer time<br>and 2004/07/8 13:35:19 in winter time.<br><br>Note that date("I") returns 1 in summer and 0 in winter.  

#

For an RFC 1123 (HTTP header date) date, try:<br><br>

```
<?php
$rfc_1123_date = gmdate(&apos;D, d M Y H:i:s T&apos;, time());
?>
```
  

#

For me most of the examples here needed the + or - seconds to set the time zone. I wanted a faster way to get the time zone in seconds. So I created this : <br>

```
<?php 
$h = "3";// Hour for time zone goes here e.g. +7 or -4, just remove the + or -
$hm = $h * 60; 
$ms = $hm * 60;
$gmdate = gmdate("m/d/Y g:i:s A", time()-($ms)); // the "-" can be switched to a plus if that&apos;s what your time zone is.
echo "Your current time now is :  $gmdate . ";
?>
```
 <br>It works. Hope it helps.  

#

[Official documentation page](https://www.php.net/manual/en/function.gmdate.php)

**[To root](/README.md)**