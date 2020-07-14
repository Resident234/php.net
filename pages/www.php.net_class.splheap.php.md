# The SplHeap class



To have a good idea what you can do with SplHeap, I created a little example script that will show the rankings of Belgian soccer teams in the Jupiler League.<br><br>

```
<?php
/**
 * A class that extends SplHeap for showing rankings in the Belgian
 * soccer tournament JupilerLeague
 */
class JupilerLeague extends SplHeap 
{
    /**
     * We modify the abstract method compare so we can sort our
     * rankings using the values of a given array
     */
    public function compare($array1, $array2)
    {
        $values1 = array_values($array1);
        $values2 = array_values($array2);
        if ($values1[0] === $values2[0]) return 0;
        return $values1[0] &lt; $values2[0] ? -1 : 1;
    }
}

// Let&apos;s populate our heap here (data of 2009)
$heap = new JupilerLeague();
$heap-&gt;insert(array (&apos;AA Gent&apos; =&gt; 15));
$heap-&gt;insert(array (&apos;Anderlecht&apos; =&gt; 20));
$heap-&gt;insert(array (&apos;Cercle Brugge&apos; =&gt; 11));
$heap-&gt;insert(array (&apos;Charleroi&apos; =&gt; 12));
$heap-&gt;insert(array (&apos;Club Brugge&apos; =&gt; 21));
$heap-&gt;insert(array (&apos;G. Beerschot&apos; =&gt; 15));
$heap-&gt;insert(array (&apos;Kortrijk&apos; =&gt; 10));
$heap-&gt;insert(array (&apos;KV Mechelen&apos; =&gt; 18));
$heap-&gt;insert(array (&apos;Lokeren&apos; =&gt; 10));
$heap-&gt;insert(array (&apos;Moeskroen&apos; =&gt; 7));
$heap-&gt;insert(array (&apos;Racing Genk&apos; =&gt; 11));
$heap-&gt;insert(array (&apos;Roeselare&apos; =&gt; 6));
$heap-&gt;insert(array (&apos;Standard&apos; =&gt; 20));
$heap-&gt;insert(array (&apos;STVV&apos; =&gt; 17));
$heap-&gt;insert(array (&apos;Westerlo&apos; =&gt; 10));
$heap-&gt;insert(array (&apos;Zulte Waregem&apos; =&gt; 15));

// For displaying the ranking we move up to the first node
$heap-&gt;top();

// Then we iterate through each node for displaying the result
while ($heap-&gt;valid()) {
  list ($team, $score) = each ($heap-&gt;current());
  echo $team . &apos;: &apos; . $score . PHP_EOL;
  $heap-&gt;next();
}
?>
```
<br><br>This results in the following output:<br>Club Brugge: 21<br>Anderlecht: 20<br>Standard: 20<br>KV Mechelen: 18<br>STVV: 17<br>Zulte Waregem: 15<br>AA Gent: 15<br>G. Beerschot: 15<br>Charleroi: 12<br>Racing Genk: 11<br>Cercle Brugge: 11<br>Kortrijk: 10<br>Lokeren: 10<br>Westerlo: 10<br>Moeskroen: 7<br>Roeselare: 6<br><br>Hope this example paved the way for more complex implementations of SplHeap.  

#

While Michelangelo Van Dam example (http://br2.php.net/manual/en/class.splheap.php#93930) is a great demonstration of what can be done with SplHeap, this implementation is exactly what SplPriorityQueue does - based on SplMaxHeap. If you&apos;re planning to copy that snippet, go no further! There&apos;s a SPL class that does exactly what you want :)  

#

[Official documentation page](https://www.php.net/manual/en/class.splheap.php)

**[To root](/README.md)**