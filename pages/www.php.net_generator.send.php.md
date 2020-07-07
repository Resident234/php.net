# Generator::send





Reading the example, it is a bit difficult to understand what exactly to do with this. The example below is a simple example of what you can do this.



```
<?php
function nums() {
&#xA0; &#xA0; for ($i = 0; $i &lt; 5; ++$i) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //get a value from the caller
&#xA0; &#xA0; &#xA0; &#xA0; $cmd = (yield $i);
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; if($cmd == &apos;stop&apos;)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return;//exit the function
&#xA0; &#xA0; &#xA0; &#xA0; }&#xA0; &#xA0;&#xA0; 
}

$gen = nums();
foreach($gen as $v)
{
&#xA0; &#xA0; if($v == 3)//we are satisfied
&#xA0; &#xA0; &#xA0; &#xA0; $gen-&gt;send(&apos;stop&apos;);
&#xA0; &#xA0; 
&#xA0; &#xA0; echo &quot;{$v}\n&quot;;
}

//Output
0
1
2
3
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/generator.send.php)

**[To root](/README.md)**