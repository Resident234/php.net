# Comments





Notes can come in all sorts of shapes and sizes. They vary, and their uses are completely up to the person writing the code. However, I try to keep things consistent in my code that way it&apos;s easy for the next person to read. So something like this might help...



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



A nice way to toggle the commenting of blocks of code can be done by mixing the two comment styles:


```
<?php
//*
if ($foo) {
&#xA0; echo $bar;
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
&#xA0; echo $bar;
}
// */
sort($morecode);
?>
```

..the block is suddenly commented out.
This works because a /* .. */ overrides //. You can even &quot;flip&quot; two blocks, like this:


```
<?php
//*
if ($foo) {
&#xA0; echo $bar;
}
/*/
if ($bar) {
&#xA0; echo $foo;
}
// */
?>
```

vs


```
<?php
/*
if ($foo) {
&#xA0; echo $bar;
}
/*/
if ($bar) {
&#xA0; echo $foo;
}
// */
?>
```



  

#



It is worth mentioning that, HTML comments have no meaning in PHP parser. So,

&lt;!-- comment


```
<?php echo some_function(); ?>
```

--&gt;

WILL execute some_function() and echo result inside HTML comment.

  

#



Comments in PHP can be used for several purposes, a very interesting one being that you can generate API documentation directly from them by using PHPDocumentor (http://www.phpdoc.org/).

Therefor one has to use a JavaDoc-like comment syntax (conforms to the DocBook DTD), example:


```
<?php
/**
* The second * here opens the DocBook commentblock, which could later on&lt;br&gt;
* in your development cycle save you a lot of time by preventing you having to rewrite&lt;br&gt;
* major documentation parts to generate some usable form of documentation.
*/
?>
```

Some basic html-like formatting is supported with this (ie &lt;br&gt; tags) to create something of a layout.

  

#



MSpreij (8-May-2005) says&#xA0; /* .. */ overrides //&#xA0; 
Anonymous (26-Jan-2006) says // overrides /* .. */

Actually, both are correct. Once a comment is opened, *everything* is ignored until the end of the comment (or the end of the php block) is reached.

Thus, if a comment is opened with: 
&#xA0;&#xA0; //&#xA0; then /* and */ are &quot;overridden&quot; until after end-of-line 
&#xA0;&#xA0; /*&#xA0; then // is &quot;overridden&quot; until after */

  

#



Be careful when commenting out regular expressions.

E.g. the following causes a parser error.

I do prefer using # as regexp delimiter anyway so it won&apos;t hurt me ;-)



```
<?php 

/*

 $f-&gt;setPattern(&apos;/^\d.*/&apos;);

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
<?php echo &apos;something&apos;; //comment ?>
```




```
<?php
if(1==1)
{
&#xA0; &#xA0; //?>
```

}
?>
```


i discovered this &quot;anomally&quot; when i commented out a line of code containing a regex which itself contained ?>
```
, with the // style comment.
e.g. //preg_match(&apos;/^(?>
```
c|b)at$/&apos;, &apos;cat&apos;, $matches);
will cause an error while commented! using /**/ style comments provides a solution. i don&apos;t know about # style comments, i don&apos;t ever personally use them.

  

#



Comments do NOT take up processing power.

So, for all the people who argue that comments are undesired because they take up processing power now have no reason to comment ;)



```
<?php

// Control
echo microtime(), &quot;&lt;br /&gt;&quot;; // 0.25163600 1292450508
echo microtime(), &quot;&lt;br /&gt;&quot;; // 0.25186000 1292450508

// Test
echo microtime(), &quot;&lt;br /&gt;&quot;; // 0.25189700 1292450508
# TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST TEST
# .. Above comment repeated 18809 times ..
echo microtime(), &quot;&lt;br /&gt;&quot;; // 0.25192100 1292450508

?>
```


They take up about the same amount of time (about meaning on a repeated testing, sometimes the difference between the control and the test was negative and sometimes positive).

  

#

[Official documentation page](https://www.php.net/manual/en/language.basic-syntax.comments.php)

**[To root](/README.md)**