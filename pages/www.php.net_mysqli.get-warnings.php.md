# mysqli::get_warnings





With a bit of rooting about with reflection, I spotted that the mysqli_warning class has a next() function, so I tried calling it and it does indeed progress through the available warnings! Following on from my earlier example:





```
<?php

$r = mysqli_query($db, &quot;INSERT INTO blah SET z = &apos;1&apos;&quot;);

$j = mysqli_warning_count($db);

if ($j &gt; 0) {

&#xA0; &#xA0; $e = mysqli_get_warnings($db);

&#xA0; &#xA0; for ($i = 0; $i &lt; $j; $i++) {

&#xA0; &#xA0; &#xA0; &#xA0; var_dump($e);

&#xA0; &#xA0; &#xA0; &#xA0; $e-&gt;next();

&#xA0; &#xA0; }

}

?>
```




There is a simple way of traversing the warnings:





```
<?php

$r = mysqli_query($db, &quot;INSERT INTO blah SET z = &apos;1&apos;&quot;);

if (mysqli_warning_count($db)) {

&#xA0;&#xA0; $e = mysqli_get_warnings($db);

&#xA0;&#xA0; do {

&#xA0; &#xA0; &#xA0;&#xA0; echo &quot;Warning: $e-&gt;errno: $e-&gt;message\n&quot;;

&#xA0;&#xA0; } while ($e-&gt;next());

}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.get-warnings.php)

**[To root](/README.md)**