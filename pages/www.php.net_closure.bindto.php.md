# Closure::bindTo



You can do pretty Javascript-like things with objects using closure binding:<br><br>

```
<?php
trait DynamicDefinition {
    
    public function __call($name, $args) {
        if (is_callable($this->$name)) {
            return call_user_func($this->$name, $args);
        }
        else {
            throw new \RuntimeException("Method {$name} does not exist");
        }
    }
    
    public function __set($name, $value) {
        $this->$name = is_callable($value)? 
            $value->bindTo($this, $this): 
            $value;
    }
}

class Foo {
    use DynamicDefinition;
    private $privateValue = 'I am private';
}

$foo = new Foo;
$foo->bar = function() {
    return $this->privateValue;
};

// prints 'I am private'
print $foo->bar();

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

        $closure = $closure->bindTo($context, $context);
        $closure($tpl);

    }

}

$art = new Article();
$post = new Post();
$template = new Template();

$template->render($art, 'tpl.php');
$template->render($post, 'tpl.php');
?>
```


#############
tpl.php
############
<h1>

```
<?php echo $this->title;?>
```
&lt;/h1&gt;  

#

Private/protected members are accessible if you set the "newscope" argument (as the manual says).<br><br>

```
<?php
$fn = function(){
    return ++$this->foo; // increase the value
};

class Bar{
    private $foo = 1; // initial value
}

$bar = new Bar();

$fn1 = $fn->bindTo($bar, 'Bar'); // specify class name
$fn2 = $fn->bindTo($bar,  $bar); // or object

echo $fn1(); // 2
echo $fn2(); // 3?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/closure.bindto.php)

**[To root](/README.md)**