# Countable::count



Even though Countable::count method is called when the object implementing Countable is used in count() function, the second parameter of count, $mode, has no influence to your class method. <br><br>$mode is not passed to  Countable::count:<br><br>

```
<?php

class Foo implements Countable
{
    public function count()
    {
        var_dump(func_get_args());
        return 1;
    }
}

count(new Foo(), COUNT_RECURSIVE);

?>
```
<br><br>var_dump output:<br><br>array(0) {<br>}  

---

[Official documentation page](https://www.php.net/manual/en/countable.count.php)

**[To root](/README.md)**