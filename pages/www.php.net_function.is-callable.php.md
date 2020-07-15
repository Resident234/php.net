# is_callable



If the target class has __call() magic function implemented, then is_callable will ALWAYS return TRUE for whatever method you call it.<br>is_callable does not evaluate your internal logic inside __call() implementation (and this is for good).<br>Therefore every method name is callable for such classes.<br><br>Hence it is WRONG to say (as someone said):<br>...is_callable will correctly determine the existence of methods made with __call...<br><br>Example:<br>

```
<?php
class TestCallable
{
    public function testing()
    {
          return "I am called.";
    }

    public function __call($name, $args)
    {
        if($name == 'testingOther') 
        {
                return call_user_func_array(array($this, 'testing'), $args);
        }
    }
}

$t = new TestCallable();
echo $t->testing();      // Output: I am called.
echo $t->testingOther(); // Output: I am called.
echo $t->working();      // Output: (null)

echo is_callable(array($t, 'testing'));       // Output: TRUE
echo is_callable(array($t, 'testingOther'));  // Output: TRUE
echo is_callable(array($t, 'working'));       // Output: TRUE, expected: FALSE
?>
```
  

#

is_callable() will try __autoload(), if have one.  

#

is_callable doesn&apos;t seem able to resolve namespaces.  If you&apos;re passing a string, then the string has to include the function&apos;s full namespace.  <br><br>

```
<?php
namespace foo\bar\baz;

function something ()
{
    return (42);
}

var_dump (is_callable ('something')); // false
var_dump (is_callable ('foo\bar\baz\something')); // true
?>
```
<br><br>It&apos;s easy to forget, but if you just prepend __NAMESPACE__ to your function name strings you should be fine in most cases.  

#

To corey at eyewantmedia dot com:<br><br>your misunderstanding lies in passing in the naked $object parameter. It is correct for is_callable to return FALSE since you cannot &apos;call an object&apos;, you can only call one of its methods, but you don&apos;t specify which one. Hence:<br><br>is_callable(array($object, &apos;some_function&apos;), [true or false], $callable_name)<br><br>will yield the correct result.<br><br>Notice, though, that a quick test I made (PHP 5.0.4) showed that is_callable incorrectly returns TRUE also if you specify the name of a protected/private method from outside of the context of the defining class, so, as wasti dot redl at gmx dot net pointed out, reflection is the way to go if you want to take visibility into account (which you should for true OOP, IMHO).  

#

[Official documentation page](https://www.php.net/manual/en/function.is-callable.php)

**[To root](/README.md)**