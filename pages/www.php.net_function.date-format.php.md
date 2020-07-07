# date_format





Requirements: PHP5+



To expand on Matt Walsh&apos;s example, a simple way to get eBay, or Amazon, web service timestamps is as follows:





```
<?php



$current_time = urlEncode(subStr(date(&quot;c&quot;), 0, 19).&quot;Z&quot;);



?>
```




In other words, take the date/time of now (in ISO 8601 format), discard the trailing Daylight Savings Time specifier, add a &quot;Z&quot; where the DST was and urlEncode the whole thing to convert the time&apos;s colons for REST requests (required for amazon, not sure about eBay).



Another way might be to create your own timestamp:





```
<?php



$current_time = urlEncode(date(&quot;Y-m-d&quot;).&quot;T&quot;.date(&quot;H:i:s&quot;).&quot;Z&quot;);



?>
```




This way however takes a little more coding on the line.



As far as performance goes, I&apos;m not sure which may be quicker. I just like things to work and work well, don&apos;t much care for how fast they are as long as they get the job done :)



A much simpler way to get the eBay, or Amazon, web service timestamp is as follows:





```
<?php



$current_date = gmDate(&quot;Y-m-d\TH:i:s\Z&quot;);



?>
```




Enjoy!

  

#

[Official documentation page](https://www.php.net/manual/en/function.date-format.php)

**[To root](/README.md)**