# The DateTime class



DateTime supports microseconds since 5.2.2. This is mentioned in the documentation for the date function, but bears repeating here. You can create a DateTime with fractional seconds and retrieve that value using the &apos;u&apos; format string.<br><br>

```
<?php
// Instantiate a DateTime with microseconds.
$d = new DateTime('2011-01-01T15:03:01.012345Z');

// Output the microseconds.
echo $d->format('u'); // 012345

// Output the date with microseconds.
echo $d->format('Y-m-d\TH:i:s.u'); // 2011-01-01T15:03:01.012345?>
```
  

#

Small but powerful extension to DateTime<br><br>

```
<?php

class Blar_DateTime extends DateTime {

    /**
     * Return Date in ISO8601 format
     *
     * @return String
     */
    public function __toString() {
        return $this->format('Y-m-d H:i');
    }

    /**
     * Return difference between $this and $now
     *
     * @param Datetime|String $now
     * @return DateInterval
     */
    public function diff($now = 'NOW') {
        if(!($now instanceOf DateTime)) {
            $now = new DateTime($now);
        }
        return parent::diff($now);
    }

    /**
     * Return Age in Years
     *
     * @param Datetime|String $now
     * @return Integer
     */
    public function getAge($now = 'NOW') {
        return $this->diff($now)->format('%y');
    }

}

?>
```


Usage:



```
<?php

$birthday = new Blar_DateTime('1879-03-14');

// Example 1
echo $birthday;
// Result: 1879-03-14 00:00

// Example 2
echo '&lt;p&gt;Albert Einstein would now be ', $birthday->getAge(), ' years old.&lt;/p&gt;';
// Result: &lt;p&gt;Albert Einstein would now be 130 years old.&lt;/p&gt;

// Example 3
echo '&lt;p&gt;Albert Einstein would now be ', $birthday->diff()->format('%y Years, %m Months, %d Days'), ' old.&lt;/p&gt;';
// Result: &lt;p&gt;Albert Einstein would now be 130 Years, 10 Months, 10 Days old.&lt;/p&gt;

// Example 4
echo '&lt;p&gt;Albert Einstein was on 2010-10-10 ', $birthday->getAge('2010-10-10'), ' years old.&lt;/p&gt;';
// Result: &lt;p&gt;Albert Einstein was on 2010-10-10 131 years old.&lt;/p&gt;

?>
```
  

#

There is a subtle difference between the following two statments which causes JavaScript&apos;s Date object on iPhones to fail.<br><br>

```
<?php
$objDateTime = new DateTime('NOW');
echo $objDateTime->format('c'); // ISO8601 formated datetime
echo $objDateTime->format(DateTime::ISO8601); // Another way to get an ISO8601 formatted string

/**
On my local machine this results in: 

2013-03-01T16:15:09+01:00
2013-03-01T16:15:09+0100

Both of these strings are valid ISO8601 datetime strings, but the latter is not accepted by the constructor of JavaScript's date object on iPhone. (Possibly other browsers as well)
*/

?>
```


Our solution was to create the following constant on our DateHelper object.



```
<?php
class DateHelper
{
    /**
     * An ISO8601 format string for PHP's date functions that's compatible with JavaScript's Date's constructor method
     * Example: 2013-04-12T16:40:00-04:00
     * 
     * PHP's ISO8601 constant doesn't add the colon to the timezone offset which is required for iPhone
    **/
    const ISO8601 = 'Y-m-d\TH:i:sP';
}
?>
```
  

#

At PHP 7.1 the DateTime constructor incorporates microseconds when constructed from the current time.  Make your comparisons carefully, since two DateTime objects constructed one after another are now more likely to have different values.<br><br>http://php.net/manual/en/migration71.incompatible.php  

#

This caused some confusion with a blog I was working on and just wanted to make other people aware of this. If you use createFromFormat to turn a date into a timestamp it will include the current time. For example:<br><br>

```
<?php
$publishDate = DateTime::createFromFormat('m/d/Y', '1/10/2014');
echo $publishDate->getTimestamp();
?>
```


Would not output the expected "1389312000" instead it would output something more like "1389344025". To fix this you would want to do:



```
<?php
$publishDate = DateTime::createFromFormat('m/d/Y', '1/10/2014');
$publishDate->setTime(0, 0, 0);
echo $publishDate->getTimestamp();
?>
```
<br><br>I hope this helps someone!  

#

IF You want to create clone of $time, use clone..<br><br>

```
<?php
  $now   = new DateTime;
  $clone = $now;        //this doesnot clone so:
  $clone->modify( '-1 day' );
 
  echo $now->format( 'd-m-Y' ), "\n", $clone->format( 'd-m-Y' );
  echo '----', "\n";

  // will print same.. if you want to clone make like this:
  $now   = new DateTime;
  $clone = clone $now;    
  $clone->modify( '-1 day' );
    
  echo $now->format( 'd-m-Y' ), "\n", $clone->format( 'd-m-Y' );
?>
```
<br><br>Results:<br>18-07-2011<br>18-07-2011<br>----<br>19-07-2011<br>18-07-2011  

#

[Official documentation page](https://www.php.net/manual/en/class.datetime.php)

**[To root](/README.md)**