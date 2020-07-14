# ReflectionMethod::getClosure



You can call private methods with getClosure():<br><br>

```
<?php

function call_private_method($object, $method, $args = array()) {
    $reflection = new ReflectionClass(get_class($object));
    $closure = $reflection-&gt;getMethod($method)-&gt;getClosure($object);
    return call_user_func_array($closure, $args);
}

class Example {

    private $x = 1, $y = 10;

    private function sum() {
        print $this-&gt;x + $this-&gt;y;
    }

}

call_private_method(new Example(), &apos;sum&apos;);

?>
```
<br><br>Output is 11.  

#

[Official documentation page](https://www.php.net/manual/en/reflectionmethod.getclosure.php)

**[To root](/README.md)**