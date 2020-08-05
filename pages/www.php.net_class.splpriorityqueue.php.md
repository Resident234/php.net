# The SplPriorityQueue class



quick implementation of SPL Priority Queue:<br><br>

```
<?php

class PQtest extends SplPriorityQueue
{
    public function compare($priority1, $priority2)
    {
        if ($priority1 === $priority2) return 0;
        return $priority1 < $priority2 ? -1 : 1;
    }
}

$objPQ = new PQtest();

$objPQ->insert('A',3);
$objPQ->insert('B',6);
$objPQ->insert('C',1);
$objPQ->insert('D',2);

echo "COUNT->".$objPQ->count()."<BR>";

//mode of extraction
$objPQ->setExtractFlags(PQtest::EXTR_BOTH);

//Go to TOP
$objPQ->top();

while($objPQ->valid()){
    print_r($objPQ->current());
    echo "<BR>";
    $objPQ->next();
}

?>
```
<br><br>output:<br><br>COUNT-&gt;4<br>Array ( [data] =&gt; B [priority] =&gt; 6 ) <br>Array ( [data] =&gt; A [priority] =&gt; 3 ) <br>Array ( [data] =&gt; D [priority] =&gt; 2 ) <br>Array ( [data] =&gt; C [priority] =&gt; 1 )  

---

[Official documentation page](https://www.php.net/manual/en/class.splpriorityqueue.php)

**[To root](/README.md)**