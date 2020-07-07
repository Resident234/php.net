# Alternative syntax for control structures





It seems to me, that many people think that



```
<?php if ($a == 5): ?>
```

A ist gleich 5


```
<?php endif; ?>
```


is only with alternate syntax possible, but 



```
<?php if ($a == 5){ ?>
```

A ist gleich 5


```
<?php }; ?>
```


is also possible.

alternate syntax makes the code only clearer and easyer to read

  

#



A simple alternative to an if statement, which is almost like a ternary operator, is the use of AND. Consider the following:



```
<?php
&#xA0; &#xA0;&#xA0; $value = &apos;Jesus&apos;;

&#xA0; &#xA0;&#xA0; // This is a simple if statement
&#xA0; &#xA0;&#xA0; if( isset( $value ) )
&#xA0; &#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; print $value;
&#xA0; &#xA0;&#xA0; }

&#xA0; &#xA0;&#xA0; print &apos;&lt;br /&gt;&apos;;

&#xA0; &#xA0;&#xA0; // This is an alternative
&#xA0; &#xA0;&#xA0; isset( $value ) AND print( $value );
?>
```


This does not work with echo() for some reason. I find this extremely useful!

  

#



isset( $value ) AND print( $value );

the reason why it doesn&apos;t work with echo, it&apos;s because echo does not return anything, while print _always_ returns 1, which is considered true in the expression

  

#



Consider the following hypothetical PHP example:



```
<?php
$bar = &apos;bar&apos;;
$foo = &apos;foo&apos;;

if (isset($bar)):
&#xA0;&#xA0; if (isset($foo)) echo &quot;Both are set.&quot;;
elseif (isset($foo)):
&#xA0;&#xA0; echo &quot;Only &apos;foo&apos; is set.&quot;;
else:
&#xA0;&#xA0; echo &quot;Only &apos;bar&apos; is set.&quot;;
endif;
?>
```


Disconsider the dumb logic and focus on the elseif line. If you try it yourself you will get a PHP EXCEPTION error saying: syntax error, unexpected &apos;:&apos; .

Now, you may think the fix is to have the sub-if enclosed in between { } instead of being a single line statement, like this:



```
<?php
$foo = &apos;foo&apos;;
$bar = &apos;bar&apos;;

if (isset($bar)):
&#xA0;&#xA0; if (isset($foo)) {
&#xA0; &#xA0;&#xA0; echo &quot;Both are set.&quot;;
&#xA0;&#xA0; }
elseif (isset($foo)):
&#xA0;&#xA0; echo &quot;Only &apos;foo&apos; is set.&quot;;
else:
&#xA0;&#xA0; echo &quot;Only &apos;bar&apos; is set.&quot;;
endif;
?>
```


Wrong! The error remains. Exactly the same EXCEPTION as before...
&#xA0; &#xA0; 
Well, here is what I found: if you put a semicolon (;) AFTER the curly bracket (}) which resides immediately before the elseif statement, then the error is gone! Try it:



```
<?php
$foo = &apos;foo&apos;;
$bar = &apos;bar&apos;;

if (isset($bar)):
&#xA0;&#xA0; if (isset($foo)) {
&#xA0; &#xA0;&#xA0; echo &quot;Both are set.&quot;;
&#xA0;&#xA0; };
elseif (isset($foo)):
&#xA0;&#xA0; echo &quot;Only &apos;foo&apos; is set.&quot;;
else:
&#xA0;&#xA0; echo &quot;Only &apos;bar&apos; is set.&quot;;
endif;
?>
```


Weird enough, if you go back to the first example and DOUBLE the semicolon immediately before the elseif statement, it will also work:



```
<?php
$foo = &apos;foo&apos;;
$bar = &apos;bar&apos;;

if (isset($bar)):
&#xA0; if (isset($foo)) echo &quot;Both are set.&quot;;;
elseif (isset($foo)):
&#xA0; echo &quot;Only &apos;foo&apos; is set.&quot;;
else:
&#xA0; echo &quot;Only &apos;bar&apos; is set.&quot;;
endif;
?>
```


But, it doesn&apos;t end there. You can also do this:



```
<?php
$foo = &apos;foo&apos;;
$bar = &apos;bar&apos;;

if (isset($bar)):
&#xA0; if (isset($foo)): echo &quot;Both are set.&quot;;
elseif (isset($foo)):
&#xA0; echo &quot;Only &apos;foo&apos; is set.&quot;;
else:
&#xA0; echo &quot;Only &apos;bar&apos; is set.&quot;;
endif;
?>
```


However, in this last example, the logic gets totally scrambled! The elseif will now belong to the sub-if instead of the first if, and the rest of the logic will all behave as a &quot;one single statement&quot; in response to the first if only. Very confusing and error prone (be careful).

The differences are very subtle and can deceive the eyes (especially while debugging). For this reason, I strongly suggest the first example from this answer: when using IF-ELSEIF blocks (AKA &quot;Alternative Syntax&quot;), if another IF is required inside it, enclose it in between {} and don&apos;t forget to add a semicolon after the last }. Example:



```
<?php
if (isset($bar)):
&#xA0;&#xA0; if (isset($foo)) {
&#xA0; &#xA0;&#xA0; echo &quot;Both are set.&quot;;
&#xA0;&#xA0; };
elseif (...):
?>
```


Maybe the truth is that someone screwed up in the language parsing process for those PHP Block Alternative Statements or failed to document this very important detail!

  

#



If you wan&apos;t to use the alternative syntax for switch statements this won&apos;t work:

&lt;div&gt;


```
<?php switch($variable): ?>
```



```
<?php case 1: ?>
```

&lt;div&gt;
Newspage
&lt;/div&gt;


```
<?php break;?>
```



```
<?php case 2: ?>
```

&lt;/div&gt;
Forum
&lt;div&gt;


```
<?php break;?>
```



```
<?php endswitch;?>
```

&lt;/div&gt;

Instead you have to workaround like this:

&lt;div&gt;


```
<?php switch($variable): 
case 1: ?>
```

&lt;div&gt;
Newspage
&lt;/div&gt;


```
<?php break;?>
```



```
<?php case 2: ?>
```

&lt;/div&gt;
Forum
&lt;div&gt;


```
<?php break;?>
```



```
<?php endswitch;?>
```

&lt;/div&gt;

  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.alternative-syntax.php)

**[To root](/README.md)**