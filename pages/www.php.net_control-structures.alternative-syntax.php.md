# Alternative syntax for control structures



It seems to me, that many people think that<br><br>

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
<br><br>is also possible.<br><br>alternate syntax makes the code only clearer and easyer to read  

#

A simple alternative to an if statement, which is almost like a ternary operator, is the use of AND. Consider the following:<br><br>

```
<?php
     $value = &apos;Jesus&apos;;

     // This is a simple if statement
     if( isset( $value ) )
     {
          print $value;
     }

     print &apos;&lt;br /&gt;&apos;;

     // This is an alternative
     isset( $value ) AND print( $value );
?>
```
<br><br>This does not work with echo() for some reason. I find this extremely useful!  

#

isset( $value ) AND print( $value );<br><br>the reason why it doesn&apos;t work with echo, it&apos;s because echo does not return anything, while print _always_ returns 1, which is considered true in the expression  

#

Consider the following hypothetical PHP example:<br><br>

```
<?php
$bar = &apos;bar&apos;;
$foo = &apos;foo&apos;;

if (isset($bar)):
   if (isset($foo)) echo "Both are set.";
elseif (isset($foo)):
   echo "Only &apos;foo&apos; is set.";
else:
   echo "Only &apos;bar&apos; is set.";
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
   if (isset($foo)) {
     echo "Both are set.";
   }
elseif (isset($foo)):
   echo "Only &apos;foo&apos; is set.";
else:
   echo "Only &apos;bar&apos; is set.";
endif;
?>
```


Wrong! The error remains. Exactly the same EXCEPTION as before...
    
Well, here is what I found: if you put a semicolon (;) AFTER the curly bracket (}) which resides immediately before the elseif statement, then the error is gone! Try it:



```
<?php
$foo = &apos;foo&apos;;
$bar = &apos;bar&apos;;

if (isset($bar)):
   if (isset($foo)) {
     echo "Both are set.";
   };
elseif (isset($foo)):
   echo "Only &apos;foo&apos; is set.";
else:
   echo "Only &apos;bar&apos; is set.";
endif;
?>
```


Weird enough, if you go back to the first example and DOUBLE the semicolon immediately before the elseif statement, it will also work:



```
<?php
$foo = &apos;foo&apos;;
$bar = &apos;bar&apos;;

if (isset($bar)):
  if (isset($foo)) echo "Both are set.";;
elseif (isset($foo)):
  echo "Only &apos;foo&apos; is set.";
else:
  echo "Only &apos;bar&apos; is set.";
endif;
?>
```


But, it doesn&apos;t end there. You can also do this:



```
<?php
$foo = &apos;foo&apos;;
$bar = &apos;bar&apos;;

if (isset($bar)):
  if (isset($foo)): echo "Both are set.";
elseif (isset($foo)):
  echo "Only &apos;foo&apos; is set.";
else:
  echo "Only &apos;bar&apos; is set.";
endif;
?>
```


However, in this last example, the logic gets totally scrambled! The elseif will now belong to the sub-if instead of the first if, and the rest of the logic will all behave as a "one single statement" in response to the first if only. Very confusing and error prone (be careful).

The differences are very subtle and can deceive the eyes (especially while debugging). For this reason, I strongly suggest the first example from this answer: when using IF-ELSEIF blocks (AKA "Alternative Syntax"), if another IF is required inside it, enclose it in between {} and don&apos;t forget to add a semicolon after the last }. Example:



```
<?php
if (isset($bar)):
   if (isset($foo)) {
     echo "Both are set.";
   };
elseif (...):
?>
```
<br><br>Maybe the truth is that someone screwed up in the language parsing process for those PHP Block Alternative Statements or failed to document this very important detail!  

#

If you wan&apos;t to use the alternative syntax for switch statements this won&apos;t work:<br><br>&lt;div&gt;<br>

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
<br>&lt;/div&gt;  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.alternative-syntax.php)

**[To root](/README.md)**