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
        return $values1[0] < $values2[0] ? -1 : 1;
    }
}

// Let's populate our heap here (data of 2009)
$heap = new JupilerLeague();
$heap->insert(array ('AA Gent' => 15));
$heap->insert(array ('Anderlecht' => 20));
$heap->insert(array ('Cercle Brugge' => 11));
$heap->insert(array ('Charleroi' => 12));
$heap->insert(array ('Club Brugge' => 21));
$heap->insert(array ('G. Beerschot' => 15));
$heap->insert(array ('Kortrijk' => 10));
$heap->insert(array ('KV Mechelen' => 18));
$heap->insert(array ('Lokeren' => 10));
$heap->insert(array ('Moeskroen' => 7));
$heap->insert(array ('Racing Genk' => 11));
$heap->insert(array ('Roeselare' => 6));
$heap->insert(array ('Standard' => 20));
$heap->insert(array ('STVV' => 17));
$heap->insert(array ('Westerlo' => 10));
$heap->insert(array ('Zulte Waregem' => 15));

// For displaying the ranking we move up to the first node
$heap->top();

// Then we iterate through each node for displaying the result
while ($heap->valid()) {
  list ($team, $score) = each ($heap->current());
  echo $team . ': ' . $score . PHP_EOL;
  $heap->next();
}
?>
```
<br><br>This results in the following output:<br>Club Brugge: 21<br>Anderlecht: 20<br>Standard: 20<br>KV Mechelen: 18<br>STVV: 17<br>Zulte Waregem: 15<br>AA Gent: 15<br>G. Beerschot: 15<br>Charleroi: 12<br>Racing Genk: 11<br>Cercle Brugge: 11<br>Kortrijk: 10<br>Lokeren: 10<br>Westerlo: 10<br>Moeskroen: 7<br>Roeselare: 6<br><br>Hope this example paved the way for more complex implementations of SplHeap.  

---

While Michelangelo Van Dam example (http://br2.php.net/manual/en/class.splheap.php#93930) is a great demonstration of what can be done with SplHeap, this implementation is exactly what SplPriorityQueue does - based on SplMaxHeap. If you&apos;re planning to copy that snippet, go no further! There&apos;s a SPL class that does exactly what you want :)  

---

[Official documentation page](https://www.php.net/manual/en/class.splheap.php)

**[To root](/README.md)**