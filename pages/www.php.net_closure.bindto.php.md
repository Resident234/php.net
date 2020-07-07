# Closure::bindTo





You can do pretty Javascript-like things with objects using closure binding:



```
<?php
trait DynamicDefinition {
&#xA0; &#xA0; 
&#xA0; &#xA0; public function __call($name, $args) {
&#xA0; &#xA0; &#xA0; &#xA0; if (is_callable($this-&gt;$name)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return call_user_func($this-&gt;$name, $args);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new \RuntimeException(&quot;Method {$name} does not exist&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; public function __set($name, $value) {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;$name = is_callable($value)? 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $value-&gt;bindTo($this, $this): 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $value;
&#xA0; &#xA0; }
}

class Foo {
&#xA0; &#xA0; use DynamicDefinition;
&#xA0; &#xA0; private $privateValue = &apos;I am private&apos;;
}

$foo = new Foo;
$foo-&gt;bar = function() {
&#xA0; &#xA0; return $this-&gt;privateValue;
};

// prints &apos;I am private&apos;
print $foo-&gt;bar();

?>
```



  

#



We can use the concept of bindTo to write a very small Template Engine:

#############
index.php
############



```
<?php

class Article{
&#xA0; &#xA0; private $title = &quot;This is an article&quot;;
}

class Post{
&#xA0; &#xA0; private $title = &quot;This is a post&quot;;
}

class Template{

&#xA0; &#xA0; function render($context, $tpl){

&#xA0; &#xA0; &#xA0; &#xA0; $closure = function($tpl){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ob_start();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; include $tpl;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return ob_end_flush();
&#xA0; &#xA0; &#xA0; &#xA0; };

&#xA0; &#xA0; &#xA0; &#xA0; $closure = $closure-&gt;bindTo($context, $context);
&#xA0; &#xA0; &#xA0; &#xA0; $closure($tpl);

&#xA0; &#xA0; }

}

$art = new Article();
$post = new Post();
$template = new Template();

$template-&gt;render($art, &apos;tpl.php&apos;);
$template-&gt;render($post, &apos;tpl.php&apos;);
?>
```


#############
tpl.php
############
&lt;h1&gt;

```
<?php echo $this-&gt;title;?>
```
&lt;/h1&gt;

  

#



Private/protected members are accessible if you set the &quot;newscope&quot; argument (as the manual says).



```
<?php
$fn = function(){
&#xA0; &#xA0; return ++$this-&gt;foo; // increase the value
};

class Bar{
&#xA0; &#xA0; private $foo = 1; // initial value
}

$bar = new Bar();

$fn1 = $fn-&gt;bindTo($bar, &apos;Bar&apos;); // specify class name
$fn2 = $fn-&gt;bindTo($bar,&#xA0; $bar); // or object

echo $fn1(); // 2
echo $fn2(); // 3


  

#

[Official documentation page](https://www.php.net/manual/en/closure.bindto.php)

**[To root](/README.md)**