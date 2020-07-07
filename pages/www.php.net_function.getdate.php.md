# getdate





Andre&apos;s code will throw an error. for the following line
&#xA0; &#xA0; 
&#xA0; &#xA0;&#xA0; $d = $todayh[mday];
&#xA0; &#xA0;&#xA0; $m = $todayh[mon];
&#xA0; &#xA0;&#xA0; $y = $todayh[year];

&quot;Notice : Undefined constant mday ,mon,year&quot;

As is, it was looking for constants called mday, mon, year etc. When it doesn&apos;t find such a constant, PHP interprets it as a string. 

like any other request it should be wrapped in quotes like this

&#xA0; &#xA0;&#xA0; $d = $todayh[&apos;mday&apos;];
&#xA0; &#xA0;&#xA0; $m = $todayh[&apos;mon&apos;];
&#xA0; &#xA0;&#xA0; $y = $todayh[&apos;year&apos;];

  

#



In addition to canby23 at ms19 post:
It&apos;s a very bad idea to consider day having 24 hours (86400 secs), because some days have 23, some - 25 hours due to daylight saving changes. Using of mkdate() and strtotime() is always preferred. strtotime() also has a very nice behaviour of datetime manipulations:


```
<?php
echo strtotime (&quot;+1 day&quot;), &quot;\n&quot;;
echo strtotime (&quot;+1 week&quot;), &quot;\n&quot;;
echo strtotime (&quot;+1 week 2 days 4 hours 2 seconds&quot;), &quot;\n&quot;;
echo strtotime (&quot;next Thursday&quot;), &quot;\n&quot;;
echo strtotime (&quot;last Monday&quot;), &quot;\n&quot;; 
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.getdate.php)

**[To root](/README.md)**