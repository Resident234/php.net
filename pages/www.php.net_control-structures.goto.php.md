# goto





Remember if you are not a fan of wild labels hanging around you are free to use braces in this construct creating a slightly cleaner look. Labels also are always executed and do not need to be called to have their associated code block ran. A purposeless example is below.





```
<?php



$headers = Array(&apos;subject&apos;, &apos;bcc&apos;, &apos;to&apos;, &apos;cc&apos;, &apos;date&apos;, &apos;sender&apos;);

$position = 0;



hIterator: {



&#xA0; &#xA0; $c = 0;

&#xA0; &#xA0; echo $headers[$position] . PHP_EOL;



&#xA0; &#xA0; cIterator: {

&#xA0; &#xA0; &#xA0; &#xA0; echo &apos; &apos; . $headers[$position][$c] . PHP_EOL;



&#xA0; &#xA0; &#xA0; &#xA0; if(!isset($headers[$position][++$c])) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; goto cIteratorExit;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; goto cIterator;

&#xA0; &#xA0; }



&#xA0; &#xA0; cIteratorExit: {

&#xA0; &#xA0; &#xA0; &#xA0; if(isset($headers[++$position])) {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; goto hIterator;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

}

?>
```



  

#



You cannot implement a Fortran-style &quot;computed GOTO&quot; in PHP because the label cannot be a variable. See: http://en.wikipedia.org/wiki/Considered_harmful





```
<?php // RAY_goto.php

error_reporting(E_ALL);



// DEMONSTRATE THAT THE GOTO LABEL IS CASE-SENSITIVE



goto a;

echo &apos;Foo&apos;;

a: echo &apos;Bar&apos;;



goto A;

echo &apos;Foo&apos;;

A: echo &apos;Baz&apos;;



// CAN THE GOTO LABEL BE A VARIABLE?



$a = &apos;abc&apos;;

goto $a; // NOPE: PARSE ERROR

echo &apos;Foo&apos;;

abc: echo &apos;Boom&apos;;

?>
```



  

#



However hated, goto is useful. When we say &quot;useful&quot; we don&apos;t mean &quot;it should be used all the time&quot; but that there are certain situations when it comes in handy.

There are times when you need a logical structure like this:


```
<?php
// ...
do {

&#xA0; &#xA0; $answer = checkFirstSource();
&#xA0; &#xA0; if(seemsGood($answer)) break;

&#xA0; &#xA0; $answer = readFromAnotherSource();
&#xA0; &#xA0; if(seemsGood($answer)) break;

&#xA0; &#xA0; // ...

}while(0);
$answer = applyFinalTouches($answer);
return $answer;
?>
```


In this case, you certainly implemented a goto with a &quot;fake loop pattern&quot;.&#xA0; It could be a lot more readable with a goto; unless, of course, you hate it.&#xA0; But the logic is clear: try everything you can to get $answer, and whenever it seems good (e.g. not empty), jump happily to the point where you format it and give it back to the caller.&#xA0; It&apos;s a proper implementation of a simple fallback mechanism.

Basically, the fight against goto is just a side effect of a misleading article many decades ago.&#xA0; Those monsters are gone now.&#xA0; Feel free to use it when you know what you&apos;re doing.

  

#



The goto operator CAN be evaluated with eval, provided the label is in the eval&apos;d code:



```
<?php
a: eval(&quot;goto a;&quot;); // undefined label &apos;a&apos;
eval(&quot;a: goto a;&quot;); // works
?>
```


It&apos;s because PHP does not consider the eval&apos;d code, containing the label, to be in the same &quot;file&quot; as the goto statement.

  

#



You are also allowed to jump backwards with a goto statement. To run a block of goto as one block is as follows:
example has a prefix of iw_ to keep label groups structured and an extra underscore to do a backwards goto.

Note the `iw_end_gt` to get out of the labels area



```
<?php
&#xA0; &#xA0; $link = true;

&#xA0; &#xA0; if ( $link ) goto iw_link_begin; 
&#xA0; &#xA0; if(false) iw__link_begin:
&#xA0; &#xA0; 
&#xA0; &#xA0; if ( $link ) goto iw_link_text;
&#xA0; &#xA0; if(false) iw__link_text:
&#xA0; &#xA0; 
&#xA0; &#xA0; if ( $link ) goto iw_link_end;
&#xA0; &#xA0; if(false) iw__link_end:
&#xA0; &#xA0; 
&#xA0; &#xA0; goto iw_end_gt;
&#xA0; &#xA0; 
&#xA0; &#xA0; 
&#xA0; &#xA0; if (false) iw_link_begin:
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;&lt;a href=&quot;#&quot;&gt;&apos;;
&#xA0; &#xA0; goto iw__link_begin;
&#xA0; &#xA0; 
&#xA0; &#xA0; if (false) iw_link_text:
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;Sample Text&apos;;
&#xA0; &#xA0; goto iw__link_text;
&#xA0; &#xA0; 
&#xA0; &#xA0; if (false) iw_link_end:
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;&lt;/a&gt;&apos;;
&#xA0; &#xA0; goto iw__link_end;
&#xA0; &#xA0; 
&#xA0; &#xA0; iw_end_gt:
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.goto.php)

**[To root](/README.md)**