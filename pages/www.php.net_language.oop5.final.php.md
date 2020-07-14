# Final Keyword



Note for Java developers: the &apos;final&apos; keyword is not used for class constants in PHP. We use the keyword &apos;const&apos;.<br><br>http://php.net/manual/en/language.oop5.constants.php  

#

Note that you cannot ovverride final methods even if they are defined as private in parent class.<br>Thus, the following example:<br>

```
<?php
class parentClass {
    final private function someMethod() { }
}
class childClass extends parentClass {
    private function someMethod() { }
}
?>
```
<br>dies with error "Fatal error: Cannot override final method parentClass::someMethod() in ***.php on line 7"<br><br>Such behaviour looks slight unexpected because in child class we cannot know, which private methods exists in a parent class and vice versa.<br><br>So, remember that if you defined a private final method, you cannot place method with the same name in child class.  

#

@thomas at somewhere dot com<br><br>The &apos;final&apos; keyword is extremely useful.  Inheritance is also useful, but can be abused and becomes problematic in large applications.  If you ever come across a finalized class or method that you wish to extend, write a decorator instead.<br><br>

```
<?php
final class Foo
{
    public method doFoo()
    {
        // do something useful and return a result
    }
}

final class FooDecorator
{
    private $foo;
    
    public function __construct(Foo $foo)
    {
        $this-&gt;foo = $foo;
    }
    
    public function doFoo()
    {
          $result = $this-&gt;foo-&gt;doFoo();
          // ... customize result ...
          return $result;
    }
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.final.php)

**[To root](/README.md)**