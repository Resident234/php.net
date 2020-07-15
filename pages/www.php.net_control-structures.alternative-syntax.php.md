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
     $value = 'Jesus';

     // This is a simple if statement
     if( isset( $value ) )
     {
          print $value;
     }

     print '<br />';

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
$bar = 'bar';
$foo = 'foo';

if (isset($bar)):
   if (isset($foo)) echo "Both are set.";
elseif (isset($foo)):
   echo "Only 'foo' is set.";
else:
   echo "Only 'bar' is set.";
endif;
?>
```


Disconsider the dumb logic and focus on the elseif line. If you try it yourself you will get a PHP EXCEPTION error saying: syntax error, unexpected ':' .

Now, you may think the fix is to have the sub-if enclosed in between { } instead of being a single line statement, like this:



```
<?php
$foo = 'foo';
$bar = 'bar';

if (isset($bar)):
   if (isset($foo)) {
     echo "Both are set.";
   }
elseif (isset($foo)):
   echo "Only 'foo' is set.";
else:
   echo "Only 'bar' is set.";
endif;
?>
```


Wrong! The error remains. Exactly the same EXCEPTION as before...
    
Well, here is what I found: if you put a semicolon (;) AFTER the curly bracket (}) which resides immediately before the elseif statement, then the error is gone! Try it:



```
<?php
$foo = 'foo';
$bar = 'bar';

if (isset($bar)):
   if (isset($foo)) {
     echo "Both are set.";
   };
elseif (isset($foo)):
   echo "Only 'foo' is set.";
else:
   echo "Only 'bar' is set.";
endif;
?>
```


Weird enough, if you go back to the first example and DOUBLE the semicolon immediately before the elseif statement, it will also work:



```
<?php
$foo = 'foo';
$bar = 'bar';

if (isset($bar)):
  if (isset($foo)) echo "Both are set.";;
elseif (isset($foo)):
  echo "Only 'foo' is set.";
else:
  echo "Only 'bar' is set.";
endif;
?>
```


But, it doesn't end there. You can also do this:



```
<?php
$foo = 'foo';
$bar = 'bar';

if (isset($bar)):
  if (isset($foo)): echo "Both are set.";
elseif (isset($foo)):
  echo "Only 'foo' is set.";
else:
  echo "Only 'bar' is set.";
endif;
?>
```


However, in this last example, the logic gets totally scrambled! The elseif will now belong to the sub-if instead of the first if, and the rest of the logic will all behave as a "one single statement" in response to the first if only. Very confusing and error prone (be careful).

The differences are very subtle and can deceive the eyes (especially while debugging). For this reason, I strongly suggest the first example from this answer: when using IF-ELSEIF blocks (AKA "Alternative Syntax"), if another IF is required inside it, enclose it in between {} and don't forget to add a semicolon after the last }. Example:



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

<div>
Newspage
</div>


```
<?php break;?>
```



```
<?php case 2: ?>
```

</div>
Forum
<div>


```
<?php break;?>
```



```
<?php endswitch;?>
```

</div>

Instead you have to workaround like this:

<div>


```
<?php switch($variable): 
case 1: ?>
```

<div>
Newspage
</div>


```
<?php break;?>
```



```
<?php case 2: ?>
```

</div>
Forum
<div>


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