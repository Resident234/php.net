# The SplPriorityQueue class



quick implementation of SPL Priority Queue:<br><br>

```
<?php

class PQtest extends SplPriorityQueue
{
    public function compare($priority1, $priority2)
    {
        if ($priority1 === $priority2) return 0;
        return $priority1 &lt; $priority2 ? -1 : 1;
    }
}

$objPQ = new PQtest();

$objPQ-&gt;insert(&apos;A&apos;,3);
$objPQ-&gt;insert(&apos;B&apos;,6);
$objPQ-&gt;insert(&apos;C&apos;,1);
$objPQ-&gt;insert(&apos;D&apos;,2);

echo "COUNT-&gt;".$objPQ-&gt;count()."&lt;BR&gt;";

//mode of extraction
$objPQ-&gt;setExtractFlags(PQtest::EXTR_BOTH);

//Go to TOP
$objPQ-&gt;top();

while($objPQ-&gt;valid()){
    print_r($objPQ-&gt;current());
    echo "&lt;BR&gt;";
    $objPQ-&gt;next();
}

?>
```
<br><br>output:<br><br>COUNT-&gt;4<br>Array ( [data] =&gt; B [priority] =&gt; 6 ) <br>Array ( [data] =&gt; A [priority] =&gt; 3 ) <br>Array ( [data] =&gt; D [priority] =&gt; 2 ) <br>Array ( [data] =&gt; C [priority] =&gt; 1 )  

#

[Official documentation page](https://www.php.net/manual/en/class.splpriorityqueue.php)

**[To root](/README.md)**