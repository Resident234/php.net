# declare





It&apos;s amazing how many people didn&apos;t grasp the concept here. Note the wording in the documentation. It states that the tick handler is called every n native execution cycles. That means native instructions, not including system calls (i&apos;m guessing). This can give you a very good idea if you need to optimize a particular part of your script, since you can measure quite effectively how many native instructions are in your actual code.

A good profiler would take that into account, and force you, the developer, to include calls to the profiler as you&apos;re entering and leaving every function. That way you&apos;d be able to keep an eye on how many cycles it took each function to complete. Independent of time.

That is extremely powerful, and not to be underestimated. A good solution would allow aggregate stats, so the total time in a function would be counted, including inside called functions.

  

#



Note that in PHP 7 

```
<?php declare(encoding=&apos;...&apos;); ?>
```
 throws an E_WARNING if Zend Multibyte is turned off.

  

#



In the following example:



```
<?php
function handler(){
&#xA0; &#xA0; print &quot;hello &lt;br /&gt;&quot;;
}

register_tick_function(&quot;handler&quot;);

declare(ticks = 1){
&#xA0; &#xA0; $b = 2;
} //closing curly bracket tickable
?>
```


&quot;Hello&quot; will be displayed twice because the closing curly bracket is also tickable. 

One may wonder why the opening curly bracket is not tickable if the closing is tickable. This is because the instruction for PHP to start ticking is given by the opening curly bracket so the ticking starts immediately after it.

  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.declare.php)

**[To root](/README.md)**