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
            throw new InvalidArgumentException(&apos;Second param must be callable&apos;);
        }
        $this-&gt;methods[$methodName] = Closure::bind($methodCallable, $this, get_class());
    }
 
    public function __call($methodName, array $args)
    {
        if (isset($this-&gt;methods[$methodName])) {
            return call_user_func_array($this-&gt;methods[$methodName], $args);
        }
 
        throw RunTimeException(&apos;There is no method with the given name to call&apos;);
    }
 
}
?>
```


test.php


```
<?php
require &apos;MetaTrait.php&apos;;
 
class HackThursday {
    use MetaTrait;
 
    private $dayOfWeek = &apos;Thursday&apos;;
 
}
 
$test = new HackThursday();
$test-&gt;addMethod(&apos;when&apos;, function () {
    return $this-&gt;dayOfWeek;
});
 
echo $test-&gt;when();

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/closure.bind.php)

**[To root](/README.md)**