# preg_replace_callback



The easiest way to pass more than one parameters to the callback function is with the &apos;use&apos; keyword. <br><br>[This is better than using global, because it works even when we are already inside a function.]<br><br>In this example, the callback function is an anonymous function, which takes one argument, $match, supplied by preg_replace_callback().  The extra <br>"use ($ten)" puts the $ten variable into scope for the function.<br><br>

```
<?php
$string = "Some numbers: one: 1; two: 2; three: 3 end";
$ten = 10;
$newstring = preg_replace_callback(
    '/(\\d+)/',
    function($match) use ($ten) { return (($match[0] + $ten)); },
    $string
    );
echo $newstring;
#prints "Some numbers: one: 11; two: 12; three: 13 end";
?>
```
  

#

If you want to call non-static function inside your class, you can do something like this. <br><br>For PHP 5.2 use second argument like array($this, &apos;replace&apos;):<br>

```
<?php
class test_preg_callback{

  private function process($text){
    $reg = "/\{([0-9a-zA-Z\- ]+)\:([0-9a-zA-Z\- ]+):?\}/";
    return preg_replace_callback($reg, array($this, 'replace'), $text);
  }
  
  private function replace($matches){
    if (method_exists($this, $matches[1])){
      return @$this->$matches[1]($matches[2]);     
    }
  }  
}
?>
```


For PHP 5.3 use second argument like "self::replace":


```
<?php
class test_preg_callback{

  private function process($text){
    $reg = "/\{([0-9a-zA-Z\- ]+)\:([0-9a-zA-Z\- ]+):?\}/";
    return preg_replace_callback($reg, "self::replace", $text);
  }
  
  private function replace($matches){
    if (method_exists($this, $matches[1])){
      return @$this->$matches[1]($matches[2]);     
    }
  }  
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.preg-replace-callback.php)

**[To root](/README.md)**