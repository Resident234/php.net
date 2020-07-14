# SplPriorityQueue::compare



At this time, the documentation sais "Note: Multiple elements with the same priority will get dequeued in no particular order."<br><br>If you need elements of equal priority to maintain insertion order, you can use something like:<br><br>

```
<?php

class StablePriorityQueue extends SplPriorityQueue {
    protected $serial = PHP_INT_MAX;
    public function insert($value, $priority) {
        parent::insert($value, array($priority, $this-&gt;serial--));
    }
}

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/splpriorityqueue.compare.php)

**[To root](/README.md)**