# gmdate





If you have the same application running in different countries, you may have some troubles getting the local time..
In my case, I was having troubles with a clock created with Macromedia Flash... the time shown by the clock was supposed to be set up by the server, passing the timestamp. When I moved the file to another country, I got a wrong time...
You can use the timezone offset ( date(&quot;Z&quot;) ) to handle this kind of thing...



```
<?php
$timestamp = time()+date(&quot;Z&quot;);
echo gmdate(&quot;Y/m/d H:i:s&quot;,$timestamp);
?>
```



  

#



Wath out for summer time and winter time...



If you want to get the current date and time based on GMT you could use this :





```
<?php

$timezone&#xA0; = -5; //(GMT -5:00) EST (U.S. &amp; Canada)

echo gmdate(&quot;Y/m/j H:i:s&quot;, time() + 3600*($timezone+date(&quot;I&quot;))); 

?>
```




this would gives: 2004/07/8 14:35:19 in summer time

and 2004/07/8 13:35:19 in winter time.



Note that date(&quot;I&quot;) returns 1 in summer and 0 in winter.

  

#



For an RFC 1123 (HTTP header date) date, try:





```
<?php

$rfc_1123_date = gmdate(&apos;D, d M Y H:i:s T&apos;, time());

?>
```



  

#



For me most of the examples here needed the + or - seconds to set the time zone. I wanted a faster way to get the time zone in seconds. So I created this : 


```
<?php 
$h = &quot;3&quot;;// Hour for time zone goes here e.g. +7 or -4, just remove the + or -
$hm = $h * 60; 
$ms = $hm * 60;
$gmdate = gmdate(&quot;m/d/Y g:i:s A&quot;, time()-($ms)); // the &quot;-&quot; can be switched to a plus if that&apos;s what your time zone is.
echo &quot;Your current time now is :&#xA0; $gmdate . &quot;;
?>
```
 
It works. Hope it helps.

  

#

[Official documentation page](https://www.php.net/manual/en/function.gmdate.php)

**[To root](/README.md)**