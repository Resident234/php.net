# ReflectionClass::getConstants



If you want to return the constants defined inside a class then you can also define an internal method as follows:<br><br>

```
<?php
class myClass {
    const NONE = 0;
    const REQUEST = 100;
    const AUTH = 101;

    // others...

    static function getConstants() {
        $oClass = new ReflectionClass(__CLASS__);
        return $oClass-&gt;getConstants();
    }
}
?>
```
  

#

You can pass $this as class for the ReflectionClass. __CLASS__ won&apos;t help if you extend the original class, because it is a magic constant based on the file itself.<br><br>

```
<?php <br><br>class Example {<br>  const TYPE_A = 1;<br>  const TYPE_B = &apos;hello&apos;;<br><br>  public function getConstants()<br>  {<br>    $reflectionClass = new ReflectionClass($this);<br>    return $reflectionClass-&gt;getConstants();<br>  }<br>}<br><br>$example = new Example();<br>var_dump($example-&gt;getConstants());<br><br>// Result:<br>array ( size = 2)<br>  &apos;TYPE_A&apos; =&gt; int 1<br>  &apos;TYPE_B&apos; =&gt; (string) &apos;hello&apos;  

#

[Official documentation page](https://www.php.net/manual/en/reflectionclass.getconstants.php)

**[To root](/README.md)**