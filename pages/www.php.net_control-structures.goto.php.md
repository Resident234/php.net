# goto



Remember if you are not a fan of wild labels hanging around you are free to use braces in this construct creating a slightly cleaner look. Labels also are always executed and do not need to be called to have their associated code block ran. A purposeless example is below.<br><br>

```
<?php

$headers = Array('subject', 'bcc', 'to', 'cc', 'date', 'sender');
$position = 0;

hIterator: {

    $c = 0;
    echo $headers[$position] . PHP_EOL;

    cIterator: {
        echo ' ' . $headers[$position][$c] . PHP_EOL;

        if(!isset($headers[$position][++$c])) {
            goto cIteratorExit;
        }
        goto cIterator;
    }

    cIteratorExit: {
        if(isset($headers[++$position])) {
            goto hIterator;
        }
    }
}
?>
```
  

#

You cannot implement a Fortran-style "computed GOTO" in PHP because the label cannot be a variable. See: http://en.wikipedia.org/wiki/Considered_harmful<br><br>

```
<?php // RAY_goto.php
error_reporting(E_ALL);

// DEMONSTRATE THAT THE GOTO LABEL IS CASE-SENSITIVE

goto a;
echo 'Foo';
a: echo 'Bar';

goto A;
echo 'Foo';
A: echo 'Baz';

// CAN THE GOTO LABEL BE A VARIABLE?

$a = 'abc';
goto $a; // NOPE: PARSE ERROR
echo 'Foo';
abc: echo 'Boom';
?>
```
  

#

However hated, goto is useful. When we say "useful" we don&apos;t mean "it should be used all the time" but that there are certain situations when it comes in handy.<br><br>There are times when you need a logical structure like this:<br>

```
<?php
// ...
do {

    $answer = checkFirstSource();
    if(seemsGood($answer)) break;

    $answer = readFromAnotherSource();
    if(seemsGood($answer)) break;

    // ...

}while(0);
$answer = applyFinalTouches($answer);
return $answer;
?>
```
<br><br>In this case, you certainly implemented a goto with a "fake loop pattern".  It could be a lot more readable with a goto; unless, of course, you hate it.  But the logic is clear: try everything you can to get $answer, and whenever it seems good (e.g. not empty), jump happily to the point where you format it and give it back to the caller.  It&apos;s a proper implementation of a simple fallback mechanism.<br><br>Basically, the fight against goto is just a side effect of a misleading article many decades ago.  Those monsters are gone now.  Feel free to use it when you know what you&apos;re doing.  

#

The goto operator CAN be evaluated with eval, provided the label is in the eval&apos;d code:<br><br>

```
<?php
a: eval("goto a;"); // undefined label 'a'
eval("a: goto a;"); // works
?>
```
<br><br>It&apos;s because PHP does not consider the eval&apos;d code, containing the label, to be in the same "file" as the goto statement.  

#

You are also allowed to jump backwards with a goto statement. To run a block of goto as one block is as follows:<br>example has a prefix of iw_ to keep label groups structured and an extra underscore to do a backwards goto.<br><br>Note the `iw_end_gt` to get out of the labels area<br><br>

```
<?php
    $link = true;

    if ( $link ) goto iw_link_begin; 
    if(false) iw__link_begin:
    
    if ( $link ) goto iw_link_text;
    if(false) iw__link_text:
    
    if ( $link ) goto iw_link_end;
    if(false) iw__link_end:
    
    goto iw_end_gt;
    
    
    if (false) iw_link_begin:
        echo '&lt;a href="#"&gt;';
    goto iw__link_begin;
    
    if (false) iw_link_text:
        echo 'Sample Text';
    goto iw__link_text;
    
    if (false) iw_link_end:
        echo '&lt;/a&gt;';
    goto iw__link_end;
    
    iw_end_gt:
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.goto.php)

**[To root](/README.md)**