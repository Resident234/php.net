# DateTime::__wakeup





If you use a version prior to 5.3 you can make __wakeup and __toString work using the following piece of code.



```
<?php
class ExtendedDateTime extends DateTime {
&#xA0; &#xA0; private $_date_time;
&#xA0; &#xA0; 
&#xA0; &#xA0; public function __toString() {
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;format(&apos;c&apos;); // format as ISO 8601
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; public function __sleep() {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;_date_time = $this-&gt;format(&apos;c&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; return array(&apos;_date_time&apos;);
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; public function __wakeup() {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;__construct($this-&gt;_date_time);
&#xA0; &#xA0; }
}
?>
```


Hope this helps someone.

  

#

[Official documentation page](https://www.php.net/manual/en/datetime.wakeup.php)

**[To root](/README.md)**