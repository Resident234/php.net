# Closure::bind





With this class and method, it&apos;s possible to do nice things, like add methods on the fly to an object.

MetaTrait.php


```
<?php
trait MetaTrait
{
&#xA0; &#xA0; 
&#xA0; &#xA0; private $methods = array();
 
&#xA0; &#xA0; public function addMethod($methodName, $methodCallable)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if (!is_callable($methodCallable)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new InvalidArgumentException(&apos;Second param must be callable&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;methods[$methodName] = Closure::bind($methodCallable, $this, get_class());
&#xA0; &#xA0; }
 
&#xA0; &#xA0; public function __call($methodName, array $args)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if (isset($this-&gt;methods[$methodName])) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return call_user_func_array($this-&gt;methods[$methodName], $args);
&#xA0; &#xA0; &#xA0; &#xA0; }
 
&#xA0; &#xA0; &#xA0; &#xA0; throw RunTimeException(&apos;There is no method with the given name to call&apos;);
&#xA0; &#xA0; }
 
}
?>
```


test.php


```
<?php
require &apos;MetaTrait.php&apos;;
 
class HackThursday {
&#xA0; &#xA0; use MetaTrait;
 
&#xA0; &#xA0; private $dayOfWeek = &apos;Thursday&apos;;
 
}
 
$test = new HackThursday();
$test-&gt;addMethod(&apos;when&apos;, function () {
&#xA0; &#xA0; return $this-&gt;dayOfWeek;
});
 
echo $test-&gt;when();

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/closure.bind.php)

**[To root](/README.md)**