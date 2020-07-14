# empty



When you need to accept these as valid, non-empty values:<br>- 0 (0 as an integer)<br>- 0.0 (0 as a float)<br>- "0" (0 as a string)<br><br>

```
<?php
function is_blank($value) {
    return empty($value) &amp;&amp; !is_numeric($value);
}
?>
```
<br><br>This is similar to Rails&apos; blank? method.  

#

Please note that results of empty() when called on non-existing / non-public variables of a class are a bit confusing if using magic method __get (as previously mentioned by nahpeps at gmx dot de). Consider this example:<br><br>

```
<?php
class Registry
{
    protected $_items = array();
    public function __set($key, $value)
    {
        $this-&gt;_items[$key] = $value;
    }
    public function __get($key)
    {
        if (isset($this-&gt;_items[$key])) {
            return $this-&gt;_items[$key];
        } else {
            return null;
        }
    }
}

$registry = new Registry();
$registry-&gt;empty = &apos;&apos;;
$registry-&gt;notEmpty = &apos;not empty&apos;;

var_dump(empty($registry-&gt;notExisting)); // true, so far so good
var_dump(empty($registry-&gt;empty)); // true, so far so good
var_dump(empty($registry-&gt;notEmpty)); // true, .. say what?
$tmp = $registry-&gt;notEmpty;
var_dump(empty($tmp)); // false as expected
?>
```


The result for empty($registry-&gt;notEmpty) is a bit unexpeced as the value is obviously set and non-empty. This is due to the fact that the empty() function uses __isset() magic functin in these cases. Although it&apos;s noted in the documentation above, I think it&apos;s worth mentioning in more detail as the behaviour is not straightforward. In order to achieve desired (expexted?) results, you need to add  __isset() magic function to your class:



```
<?php
class Registry
{
    protected $_items = array();
    public function __set($key, $value)
    {
        $this-&gt;_items[$key] = $value;
    }
    public function __get($key)
    {
        if (isset($this-&gt;_items[$key])) {
            return $this-&gt;_items[$key];
        } else {
            return null;
        }
    }
    public function __isset($key)
    {
        if (isset($this-&gt;_items[$key])) {
            return (false === empty($this-&gt;_items[$key]));
        } else {
            return null;
        }
    }
}

$registry = new Registry();
$registry-&gt;empty = &apos;&apos;;
$registry-&gt;notEmpty = &apos;not empty&apos;;

var_dump(empty($registry-&gt;notExisting)); // true, so far so good
var_dump(empty($registry-&gt;empty)); // true, so far so good
var_dump(empty($registry-&gt;notEmpty)); // false, finally!
?>
```
<br><br>It actually seems that empty() is returning negation of the __isset() magic function result, hence the negation of the empty() result in the __isset() function above.  

#

I&apos;m summarising a few points on empty() with inaccessible properties, in the hope of saving others a bit of time. Using PHP 5.3.2.<br>

```
<?php
class MyClass {
    private $foo = &apos;foo&apos;;
}
$myClass = new MyClass;
echo $myClass-&gt;foo;
?>
```

As expected, this gives "Fatal error: Cannot access private property MyClass::$foo".
But substitute the line
if (empty($myClass-&gt;foo)) echo &apos;foo is empty&apos;; else echo &apos;foo is not empty&apos;;
and we get the misleading result "foo is empty". 
There is NO ERROR OR WARNING, so this is a real gotcha. Your code will just go wrong silently, and I would say it amounts to a bug.
If you add two magic functions to the class:
public function __get($var) { return $this-&gt;$var; }
public function __isset($var) { return isset($this-&gt;$var); }
then we get the expected result. You need both functions.
For empty($myClass-&gt;foo), I believe PHP calls __isset, and if that is true returns the result of empty on the result of __get. (Some earlier posts wrongly suggest PHP just returns the negation of __isset).
BUT &#x2026;
See the earlier post by php at lanar dot com. I confirm those results, and if you extend the test with isset($x-&gt;a-&gt;b-&gt;c) it appears that __isset is only called for the last property in the chain. Arguably another bug. empty() behaves in the same way. So things are not as clear as we might hope.
See also the note on empty() at
http://uk3.php.net/manual/en/language.oop5.overloading.php<br>Clear as mud!  

#

To add on to what anon said, what&apos;s happening in john_jian&apos;s example seems unusual because we don&apos;t see the implicit typecasting going on behind the scenes.  What&apos;s really happening is:<br><br>$a = &apos;&apos;;<br>$b = 0;<br>$c = &apos;0&apos;;<br><br>(int)$a == $b -&gt; true, because any string that&apos;s not a number gets converted to 0<br>$b==(int)$c -&gt; true, because the int in the string gets converted<br>and<br>$a==$c -&gt; false, because they&apos;re being compared as strings, rather than integers.  (int)$a==(int)$c should return true, however.<br><br>Note: I don&apos;t remember if PHP even *has* typecasting, much less if this is the correct syntax.  I&apos;m just using something for the sake of examples.  

#



```
<?php
$str = &apos;            &apos;;
var_dump(empty($str)); // boolean false
?>
```


So remember to trim your strings first!



```
<?php
$str = &apos;        &apos;;
$str = trim($str);
var_dump(empty($str)); // boolean true
?>
```
  

#

test if all multiarray&apos;s are empty<br><br>

```
<?php
function is_multiArrayEmpty($multiarray) {
    if(is_array($multiarray) and !empty($multiarray)){
        $tmp = array_shift($multiarray);
            if(!is_multiArrayEmpty($multiarray) or !is_multiArrayEmpty($tmp)){
                return false;
            }
            return true;
    }
    if(empty($multiarray)){
        return true;
    }
    return false;
}

$testCase = array (     
0 =&gt; &apos;&apos;,
1 =&gt; "",
2 =&gt; null,
3 =&gt; array(),
4 =&gt; array(array()),
5 =&gt; array(array(array(array(array())))),
6 =&gt; array(array(), array(), array(), array(), array()),
7 =&gt; array(array(array(), array()), array(array(array(array(array(array(), array())))))),
8 =&gt; array(null),
9 =&gt; &apos;not empty&apos;,
10 =&gt; "not empty",
11 =&gt; array(array("not empty")),
12 =&gt; array(array(),array("not empty"),array(array()))
);

foreach ($testCase as $key =&gt; $case ) {
    echo "$key is_multiArrayEmpty= ".is_multiArrayEmpty($case)."&lt;br&gt;";
}
?>
```
<br><br>OUTPUT:<br>========<br><br>0 is_multiArrayEmpty= 1<br>1 is_multiArrayEmpty= 1<br>2 is_multiArrayEmpty= 1<br>3 is_multiArrayEmpty= 1<br>4 is_multiArrayEmpty= 1<br>5 is_multiArrayEmpty= 1<br>6 is_multiArrayEmpty= 1<br>7 is_multiArrayEmpty= 1<br>8 is_multiArrayEmpty= 1<br>9 is_multiArrayEmpty= <br>10 is_multiArrayEmpty= <br>11 is_multiArrayEmpty= <br>12 is_multiArrayEmpty=  

#

[Official documentation page](https://www.php.net/manual/en/function.empty.php)

**[To root](/README.md)**