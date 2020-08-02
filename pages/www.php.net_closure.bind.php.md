# Closure::bind



With this class and method, it&apos;s possible to do nice things, like add methods on the fly to an object.<br><br>MetaTrait.php<br>

```
<?php
trait MetaTrait
{
    
    private $methods = array();
 
    public function addMethod($methodName, $methodCallable)
    {
        if (!is_callable($methodCallable)) {
            throw new InvalidArgumentException('Second param must be callable');
        }
        $this->methods[$methodName] = Closure::bind($methodCallable, $this, get_class());
    }
 
    public function __call($methodName, array $args)
    {
        if (isset($this->methods[$methodName])) {
            return call_user_func_array($this->methods[$methodName], $args);
        }
 
        throw RunTimeException('There is no method with the given name to call');
    }
 
}
?>
```


test.php


```
<?php
require 'MetaTrait.php';
 
class HackThursday {
    use MetaTrait;
 
    private $dayOfWeek = 'Thursday';
 
}
 
$test = new HackThursday();
$test->addMethod('when', function () {
    return $this->dayOfWeek;
});
 
echo $test->when();

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/closure.bind.php)

**[To root](/README.md)**