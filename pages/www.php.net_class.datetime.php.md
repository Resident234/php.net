# The DateTime class





DateTime supports microseconds since 5.2.2. This is mentioned in the documentation for the date function, but bears repeating here. You can create a DateTime with fractional seconds and retrieve that value using the &apos;u&apos; format string.



```
<?php
// Instantiate a DateTime with microseconds.
$d = new DateTime(&apos;2011-01-01T15:03:01.012345Z&apos;);

// Output the microseconds.
echo $d-&gt;format(&apos;u&apos;); // 012345

// Output the date with microseconds.
echo $d-&gt;format(&apos;Y-m-d\TH:i:s.u&apos;); // 2011-01-01T15:03:01.012345


  

#



Small but powerful extension to DateTime



```
<?php

class Blar_DateTime extends DateTime {

&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Return Date in ISO8601 format
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; * @return String
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public function __toString() {
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;format(&apos;Y-m-d H:i&apos;);
&#xA0; &#xA0; }

&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Return difference between $this and $now
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; * @param Datetime|String $now
&#xA0; &#xA0;&#xA0; * @return DateInterval
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public function diff($now = &apos;NOW&apos;) {
&#xA0; &#xA0; &#xA0; &#xA0; if(!($now instanceOf DateTime)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $now = new DateTime($now);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; return parent::diff($now);
&#xA0; &#xA0; }

&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * Return Age in Years
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; * @param Datetime|String $now
&#xA0; &#xA0;&#xA0; * @return Integer
&#xA0; &#xA0;&#xA0; */
&#xA0; &#xA0; public function getAge($now = &apos;NOW&apos;) {
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;diff($now)-&gt;format(&apos;%y&apos;);
&#xA0; &#xA0; }

}

?>
```


Usage:



```
<?php

$birthday = new Blar_DateTime(&apos;1879-03-14&apos;);

// Example 1
echo $birthday;
// Result: 1879-03-14 00:00

// Example 2
echo &apos;&lt;p&gt;Albert Einstein would now be &apos;, $birthday-&gt;getAge(), &apos; years old.&lt;/p&gt;&apos;;
// Result: &lt;p&gt;Albert Einstein would now be 130 years old.&lt;/p&gt;

// Example 3
echo &apos;&lt;p&gt;Albert Einstein would now be &apos;, $birthday-&gt;diff()-&gt;format(&apos;%y Years, %m Months, %d Days&apos;), &apos; old.&lt;/p&gt;&apos;;
// Result: &lt;p&gt;Albert Einstein would now be 130 Years, 10 Months, 10 Days old.&lt;/p&gt;

// Example 4
echo &apos;&lt;p&gt;Albert Einstein was on 2010-10-10 &apos;, $birthday-&gt;getAge(&apos;2010-10-10&apos;), &apos; years old.&lt;/p&gt;&apos;;
// Result: &lt;p&gt;Albert Einstein was on 2010-10-10 131 years old.&lt;/p&gt;

?>
```



  

#



There is a subtle difference between the following two statments which causes JavaScript&apos;s Date object on iPhones to fail.



```
<?php
$objDateTime = new DateTime(&apos;NOW&apos;);
echo $objDateTime-&gt;format(&apos;c&apos;); // ISO8601 formated datetime
echo $objDateTime-&gt;format(DateTime::ISO8601); // Another way to get an ISO8601 formatted string

/**
On my local machine this results in: 

2013-03-01T16:15:09+01:00
2013-03-01T16:15:09+0100

Both of these strings are valid ISO8601 datetime strings, but the latter is not accepted by the constructor of JavaScript&apos;s date object on iPhone. (Possibly other browsers as well)
*/

?>
```


Our solution was to create the following constant on our DateHelper object.



```
<?php
class DateHelper
{
&#xA0; &#xA0; /**
&#xA0; &#xA0;&#xA0; * An ISO8601 format string for PHP&apos;s date functions that&apos;s compatible with JavaScript&apos;s Date&apos;s constructor method
&#xA0; &#xA0;&#xA0; * Example: 2013-04-12T16:40:00-04:00
&#xA0; &#xA0;&#xA0; * 
&#xA0; &#xA0;&#xA0; * PHP&apos;s ISO8601 constant doesn&apos;t add the colon to the timezone offset which is required for iPhone
&#xA0; &#xA0; **/
&#xA0; &#xA0; const ISO8601 = &apos;Y-m-d\TH:i:sP&apos;;
}
?>
```



  

#



At PHP 7.1 the DateTime constructor incorporates microseconds when constructed from the current time.&#xA0; Make your comparisons carefully, since two DateTime objects constructed one after another are now more likely to have different values.

http://php.net/manual/en/migration71.incompatible.php

  

#



This caused some confusion with a blog I was working on and just wanted to make other people aware of this. If you use createFromFormat to turn a date into a timestamp it will include the current time. For example:



```
<?php
$publishDate = DateTime::createFromFormat(&apos;m/d/Y&apos;, &apos;1/10/2014&apos;);
echo $publishDate-&gt;getTimestamp();
?>
```


Would not output the expected &quot;1389312000&quot; instead it would output something more like &quot;1389344025&quot;. To fix this you would want to do:



```
<?php
$publishDate = DateTime::createFromFormat(&apos;m/d/Y&apos;, &apos;1/10/2014&apos;);
$publishDate-&gt;setTime(0, 0, 0);
echo $publishDate-&gt;getTimestamp();
?>
```


I hope this helps someone!

  

#



IF You want to create clone of $time, use clone..



```
<?php
&#xA0; $now&#xA0;&#xA0; = new DateTime;
&#xA0; $clone = $now;&#xA0; &#xA0; &#xA0; &#xA0; //this doesnot clone so:
&#xA0; $clone-&gt;modify( &apos;-1 day&apos; );
 
&#xA0; echo $now-&gt;format( &apos;d-m-Y&apos; ), &quot;\n&quot;, $clone-&gt;format( &apos;d-m-Y&apos; );
&#xA0; echo &apos;----&apos;, &quot;\n&quot;;

&#xA0; // will print same.. if you want to clone make like this:
&#xA0; $now&#xA0;&#xA0; = new DateTime;
&#xA0; $clone = clone $now;&#xA0; &#xA0; 
&#xA0; $clone-&gt;modify( &apos;-1 day&apos; );
&#xA0; &#xA0; 
&#xA0; echo $now-&gt;format( &apos;d-m-Y&apos; ), &quot;\n&quot;, $clone-&gt;format( &apos;d-m-Y&apos; );
?>
```


Results:
18-07-2011
18-07-2011
----
19-07-2011
18-07-2011

  

#

[Official documentation page](https://www.php.net/manual/en/class.datetime.php)

**[To root](/README.md)**