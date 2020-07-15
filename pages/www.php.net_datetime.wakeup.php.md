# DateTime::__wakeup



If you use a version prior to 5.3 you can make __wakeup and __toString work using the following piece of code.<br><br>

```
<?php
class ExtendedDateTime extends DateTime {
    private $_date_time;
    
    public function __toString() {
        return $this->format('c'); // format as ISO 8601
    }
    
    public function __sleep() {
        $this->_date_time = $this->format('c');
        return array('_date_time');
    }
    
    public function __wakeup() {
        $this->__construct($this->_date_time);
    }
}
?>
```
<br><br>Hope this helps someone.  

#

[Official documentation page](https://www.php.net/manual/en/datetime.wakeup.php)

**[To root](/README.md)**