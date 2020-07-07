# Final Keyword





Note for Java developers: the &apos;final&apos; keyword is not used for class constants in PHP. We use the keyword &apos;const&apos;.

http://php.net/manual/en/language.oop5.constants.php

  

#



Note that you cannot ovverride final methods even if they are defined as private in parent class.

Thus, the following example:



```
<?php

class parentClass {

&#xA0; &#xA0; final private function someMethod() { }

}

class childClass extends parentClass {

&#xA0; &#xA0; private function someMethod() { }

}

?>
```


dies with error &quot;Fatal error: Cannot override final method parentClass::someMethod() in ***.php on line 7&quot;



Such behaviour looks slight unexpected because in child class we cannot know, which private methods exists in a parent class and vice versa.



So, remember that if you defined a private final method, you cannot place method with the same name in child class.

  

#



@thomas at somewhere dot com

The &apos;final&apos; keyword is extremely useful.&#xA0; Inheritance is also useful, but can be abused and becomes problematic in large applications.&#xA0; If you ever come across a finalized class or method that you wish to extend, write a decorator instead.



```
<?php
final class Foo
{
&#xA0; &#xA0; public method doFoo()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; // do something useful and return a result
&#xA0; &#xA0; }
}

final class FooDecorator
{
&#xA0; &#xA0; private $foo;
&#xA0; &#xA0; 
&#xA0; &#xA0; public function __construct(Foo $foo)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;foo = $foo;
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; public function doFoo()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $result = $this-&gt;foo-&gt;doFoo();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // ... customize result ...
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $result;
&#xA0; &#xA0; }
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.final.php)

**[To root](/README.md)**