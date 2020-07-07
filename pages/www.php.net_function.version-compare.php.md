# version_compare





[editors note]

snipbit fixed after comment from Matt Mullenweg



--jm

[/editors note]



so in a nutshell... I believe it works best like this:





```
<?php

if (version_compare(phpversion(), &quot;4.3.0&quot;, &quot;&gt;=&quot;)) {

&#xA0; // you&apos;re on 4.3.0 or later

} else {

&#xA0; // you&apos;re not

}

php?>
```



  

#



Since this function considers 1 &lt; 1.0 &lt; 1.0.0, others might find this function useful (which considers 1 == 1.0):





```
<?php

//Compare two sets of versions, where major/minor/etc. releases are separated by dots.

//Returns 0 if both are equal, 1 if A &gt; B, and -1 if B &lt; A.

function version_compare2($a, $b)

{

&#xA0; &#xA0; $a = explode(&quot;.&quot;, rtrim($a, &quot;.0&quot;)); //Split version into pieces and remove trailing .0

&#xA0; &#xA0; $b = explode(&quot;.&quot;, rtrim($b, &quot;.0&quot;)); //Split version into pieces and remove trailing .0

&#xA0; &#xA0; foreach ($a as $depth =&gt; $aVal)

&#xA0; &#xA0; { //Iterate over each piece of A

&#xA0; &#xA0; &#xA0; &#xA0; if (isset($b[$depth]))

&#xA0; &#xA0; &#xA0; &#xA0; { //If B matches A to this depth, compare the values

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($aVal &gt; $b[$depth]) return 1; //Return A &gt; B

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else if ($aVal &lt; $b[$depth]) return -1; //Return B &gt; A

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //An equal result is inconclusive at this point

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; else

&#xA0; &#xA0; &#xA0; &#xA0; { //If B does not match A to this depth, then A comes after B in sort order

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return 1; //so return A &gt; B

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; }

&#xA0; &#xA0; //At this point, we know that to the depth that A and B extend to, they are equivalent.

&#xA0; &#xA0; //Either the loop ended because A is shorter than B, or both are equal.

&#xA0; &#xA0; return (count($a) &lt; count($b)) ? -1 : 0;

}

php?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.version-compare.php)

**[To root](/README.md)**