# The Countable interface



I just want to point out that your class has to actually implement the Countable interface, not just define a count method, to be able to use count($object) and get the expected results. I.e. the first example below won&apos;t work as expected, the second will. (The normal arrow function accessor ($object-&gt;count()) will work fine, but that&apos;s not the kewl part :) )<br><br>

```
<?php
//Example One, BAD :(

class CountMe
{

    protected $_myCount = 3;

    public function count()
    {
        return $this->_myCount;
    }

}

$countable = new CountMe();
echo count($countable); //result is "1", not as expected

//Example Two, GOOD :)

class CountMe implements Countable
{

    protected $_myCount = 3;

    public function count()
    {
        return $this->_myCount;
    }

}

$countable = new CountMe();
echo count($countable); //result is "3" as expected
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.countable.php)

**[To root](/README.md)**