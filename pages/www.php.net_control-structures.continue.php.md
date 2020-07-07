# continue





The remark &quot;in PHP the switch statement is considered a looping structure for the purposes of continue&quot; near the top of this page threw me off, so I experimented a little using the following code to figure out what the exact semantics of continue inside a switch is:



```
<?php

&#xA0; &#xA0; for( $i = 0; $i &lt; 3; ++ $i )
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos; [&apos;, $i, &apos;] &apos;;
&#xA0; &#xA0; &#xA0; &#xA0; switch( $i )
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case 0: echo &apos;zero&apos;; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case 1: echo &apos;one&apos; ; XXXX;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case 2: echo &apos;two&apos; ; break;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos; &lt;&apos; , $i, &apos;&gt; &apos;;
&#xA0; &#xA0; }

?>
```


For XXXX I filled in

- continue 1
- continue 2
- break 1
- break 2

and observed the different results.&#xA0; This made me come up with the following one-liner that describes the difference between break and continue:

continue resumes execution just before the closing curly bracket ( } ), and break resumes execution just after the closing curly bracket.

Corollary: since a switch is not (really) a looping structure, resuming execution just before a switch&apos;s closing curly bracket has the same effect as using a break statement.&#xA0; In the case of (for, while, do-while) loops, resuming execution just prior their closing curly brackets means that a new iteration is started --which is of course very unlike the behavior of a break statement.

In the one-liner above I ignored the existence of parameters to break/continue, but the one-liner is also valid when parameters are supplied.

  

#



Using continue and break:





```
<?php

$stack = array(&apos;first&apos;, &apos;second&apos;, &apos;third&apos;, &apos;fourth&apos;, &apos;fifth&apos;);



foreach($stack AS $v){

&#xA0; &#xA0; if($v == &apos;second&apos;)continue;

&#xA0; &#xA0; if($v == &apos;fourth&apos;)break;

&#xA0; &#xA0; echo $v.&apos;&lt;br&gt;&apos;;

}

/*



first

third



*/



$stack2 = array(&apos;one&apos;=&gt;&apos;first&apos;, &apos;two&apos;=&gt;&apos;second&apos;, &apos;three&apos;=&gt;&apos;third&apos;, &apos;four&apos;=&gt;&apos;fourth&apos;, &apos;five&apos;=&gt;&apos;fifth&apos;);

foreach($stack2 AS $k=&gt;$v){

&#xA0; &#xA0; if($v == &apos;second&apos;)continue;

&#xA0; &#xA0; if($k == &apos;three&apos;)continue;

&#xA0; &#xA0; if($v == &apos;fifth&apos;)break;

&#xA0; &#xA0; echo $k.&apos; ::: &apos;.$v.&apos;&lt;br&gt;&apos;;

}

/*



one ::: first

four ::: fourth



*/



?>
```



  

#



If you use a incrementing value in your loop, be sure to increment it before calling continue; or you might get an infinite loop.

  

#



The most basic example that print &quot;13&quot;, skipping over 2.



```
<?php
$arr = array(1, 2, 3);
foreach($arr as $number) {
&#xA0; if($number == 2) {
&#xA0; &#xA0; continue;
&#xA0; }
&#xA0; print $number;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.continue.php)

**[To root](/README.md)**