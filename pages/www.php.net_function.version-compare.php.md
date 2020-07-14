# version_compare



[editors note]<br>snipbit fixed after comment from Matt Mullenweg<br><br>--jm<br>[/editors note]<br><br>so in a nutshell... I believe it works best like this:<br><br>

```
<?php
if (version_compare(phpversion(), "4.3.0", "&gt;=")) {
  // you're on 4.3.0 or later
} else {
  // you're not
}
?>
```
  

#

Since this function considers 1 &lt; 1.0 &lt; 1.0.0, others might find this function useful (which considers 1 == 1.0):<br><br>

```
<?php
//Compare two sets of versions, where major/minor/etc. releases are separated by dots.
//Returns 0 if both are equal, 1 if A &gt; B, and -1 if B &lt; A.
function version_compare2($a, $b)
{
    $a = explode(".", rtrim($a, ".0")); //Split version into pieces and remove trailing .0
    $b = explode(".", rtrim($b, ".0")); //Split version into pieces and remove trailing .0
    foreach ($a as $depth => $aVal)
    { //Iterate over each piece of A
        if (isset($b[$depth]))
        { //If B matches A to this depth, compare the values
            if ($aVal &gt; $b[$depth]) return 1; //Return A &gt; B
            else if ($aVal &lt; $b[$depth]) return -1; //Return B &gt; A
            //An equal result is inconclusive at this point
        }
        else
        { //If B does not match A to this depth, then A comes after B in sort order
            return 1; //so return A &gt; B
        }
    }
    //At this point, we know that to the depth that A and B extend to, they are equivalent.
    //Either the loop ended because A is shorter than B, or both are equal.
    return (count($a) &lt; count($b)) ? -1 : 0;
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.version-compare.php)

**[To root](/README.md)**