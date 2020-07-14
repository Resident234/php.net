# Closure::bindTo



You can do pretty Javascript-like things with objects using closure binding:<br><br>

```
<?php
trait DynamicDefinition {
    
    public function __call($name, $args) {
        if (is_callable($this-&gt;$name)) {
            return call_user_func($this-&gt;$name, $args);
        }
        else {
            throw new \RuntimeException("Method {$name} does not exist");
        }
    }
    
    public function __set($name, $value) {
        $this-&gt;$name = is_callable($value)? 
            $value-&gt;bindTo($this, $this): 
            $value;
    }
}

class Foo {
    use DynamicDefinition;
    private $privateValue = &apos;I am private&apos;;
}

$foo = new Foo;
$foo-&gt;bar = function() {
    return $this-&gt;privateValue;
};

// prints &apos;I am private&apos;
print $foo-&gt;bar();

?>
```
  

#

We can use the concept of bindTo to write a very small Template Engine:<br><br>#############<br>index.php<br>############<br><br>

```
<?php

class Article{
    private $title = "This is an article";
}

class Post{
    private $title = "This is a post";
}

class Template{

    function render($context, $tpl){

        $closure = function($tpl){
            ob_start();
            include $tpl;
            return ob_end_flush();
        };

        $closure = $closure-&gt;bindTo($context, $context);
        $closure($tpl);

    }

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

Private/protected members are accessible if you set the "newscope" argument (as the manual says).<br><br>

```
<?php<br>$fn = function(){<br>    return ++$this-&gt;foo; // increase the value<br>};<br><br>class Bar{<br>    private $foo = 1; // initial value<br>}<br><br>$bar = new Bar();<br><br>$fn1 = $fn-&gt;bindTo($bar, &apos;Bar&apos;); // specify class name<br>$fn2 = $fn-&gt;bindTo($bar,  $bar); // or object<br><br>echo $fn1(); // 2<br>echo $fn2(); // 3  

#

[Official documentation page](https://www.php.net/manual/en/closure.bindto.php)

**[To root](/README.md)**