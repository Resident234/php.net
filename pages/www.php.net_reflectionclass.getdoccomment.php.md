# ReflectionClass::getDocComment



According to what I can find in the PHP (5.3.2) source code, getDocComment will return the doc comment as the parser found it.<br>The doc comment (T_DOC_COMMENT) must begin with a /** - that&apos;s two asterisks, not one. The comment continues until the first */. A normal multi-line comment /*...*/ (T_COMMENT) does not count as a doc comment.<br><br>The doc comment itself includes those five characters, so 

```
<?php substr($doccomment, 3, -2) ?>
```
 will get you what&apos;s inside. A call to trim() after is recommended.  

#

If you&apos;re using a bytecode cache like eAccelerator this method will return FALSE even if there is a properly formatted Docblock. It looks like the information required by this method gets stripped out by the bytecode cache.  

#

You can also get the docblock definitions for the defined methods of a class as such:<br><br>

```
<?php
/**
 * This is an Example class
 */
class Example
{
    /**
     * This is an example function
     */
    public function fn() 
    {
        // void
    }
}

$reflector = new ReflectionClass('Example');

// to get the Class DocBlock
echo $reflector->getDocComment()

// to get the Method DocBlock
$reflector->getMethod('fn')->getDocComment();

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/reflectionclass.getdoccomment.php)

**[To root](/README.md)**