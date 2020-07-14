# Comments



Notes can come in all sorts of shapes and sizes. They vary, and their uses are completely up to the person writing the code. However, I try to keep things consistent in my code that way it&apos;s easy for the next person to read. So something like this might help...<br><br>

```
<?php

//======================================================================
// CATEGORY LARGE FONT
//======================================================================

//-----------------------------------------------------
// Sub-Category Smaller Font
//-----------------------------------------------------

/* Title Here Notice the First Letters are Capitalized */

# Option 1
# Option 2
# Option 3

/*
 * This is a detailed explanation
 * of something that should require
 * several paragraphs of information.
 */
 
// This is a single line quote.
?>
```
  

#

A nice way to toggle the commenting of blocks of code can be done by mixing the two comment styles:<br>

```
<?php
//*
if ($foo) {
  echo $bar;
}
// */
sort($morecode);
?>
```


Now by taking out one / on the first line..



```
<?php
/*
if ($foo) {
  echo $bar;
}
// */
sort($morecode);
?>
```

..the block is suddenly commented out.
This works because a /* .. */ overrides //. You can even "flip" two blocks, like this:


```
<?php
//*
if ($foo) {
  echo $bar;
}
/*/
if ($bar) {
  echo $foo;
}
// */
?>
```

vs


```
<?php
/*
if ($foo) {
  echo $bar;
}
/*/
if ($bar) {
  echo $foo;
}
// */
?>
```
  

#

It is worth mentioning that, HTML comments have no meaning in PHP parser. So,<br><br>&lt;!-- comment<br>

```
<?php echo some_function(); ?>
```
<br>--&gt;<br><br>WILL execute some_function() and echo result inside HTML comment.  

#

Comments in PHP can be used for several purposes, a very interesting one being that you can generate API documentation directly from them by using PHPDocumentor (http://www.phpdoc.org/).<br><br>Therefor one has to use a JavaDoc-like comment syntax (conforms to the DocBook DTD), example:<br>

```
<?php
/**
* The second * here opens the DocBook commentblock, which could later on&lt;br&gt;
* in your development cycle save you a lot of time by preventing you having to rewrite&lt;br&gt;
* major documentation parts to generate some usable form of documentation.
*/
?>
```
<br>Some basic html-like formatting is supported with this (ie &lt;br&gt; tags) to create something of a layout.  

#

MSpreij (8-May-2005) says  /* .. */ overrides //  <br>Anonymous (26-Jan-2006) says // overrides /* .. */<br><br>Actually, both are correct. Once a comment is opened, *everything* is ignored until the end of the comment (or the end of the php block) is reached.<br><br>Thus, if a comment is opened with: <br>   //  then /* and */ are "overridden" until after end-of-line <br>   /*  then // is "overridden" until after */  

#

Be careful when commenting out regular expressions.<br><br>E.g. the following causes a parser error.<br><br>I do prefer using # as regexp delimiter anyway so it won&apos;t hurt me ;-)<br><br>

```
<?php 

/*

 $f->setPattern('/^\d.*/');

*/

?>
```
  

#

it&apos;s perhaps not obvious to some, but the following code will cause a parse error! the ?>
```
 in //?>
```
 is not treated as commented text, this is a result of having to handle code on one line such as 

```
<?php echo 'something'; //comment ?>
```




```
<?php
if(1==1)
{
    //?>
```

}
?>
```


i discovered this "anomally" when i commented out a line of code containing a regex which itself contained ?>
```
, with the // style comment.
e.g. //preg_match('/^(?>
```
c|b)at$/&apos;, &apos;cat&apos;, $matches);<br>will cause an error while commented! using /**/ style comments provides a solution. i don&apos;t know about # style comments, i don&apos;t ever personally use them.  

#

Comments do NOT take up processing power.<br><br>So, for all the people who argue that comments are undesired because they take up processing power now have no reason to comment ;)<br><br>

```
<?php

// Control
echo microtime(), "&lt;br /&gt;"; // 0.25163600 1292450508
echo microtime(), "&lt;br /&gt;"; // 0.25186000 1292450508

// Test
echo microtime(), "&lt;br /&gt;"; // 0.25189700 1292450508
# TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST
# .. Above comment repeated 18809 times ..
echo microtime(), "&lt;br /&gt;"; // 0.25192100 1292450508

?>
```
<br><br>They take up about the same amount of time (about meaning on a repeated testing, sometimes the difference between the control and the test was negative and sometimes positive).  

#

[Official documentation page](https://www.php.net/manual/en/language.basic-syntax.comments.php)

**[To root](/README.md)**