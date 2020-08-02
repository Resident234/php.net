# mysqli_result::fetch_all



I tested using "fetch all" versus "while / fetch array" and :<br><br>fetch-all uses less memory (but not for so much).<br><br>In my case (test1 and test2): 147008,262848 bytes (fetch-all) versus 147112,262888 bytes (fetch-array &amp; while.<br><br>So, about the memory, in both cases are the same.<br><br>However, about the performance<br>My test takes :350ms (worst case) using fetch-all, while it takes 464ms (worst case) using fetch-array, or about 35% worst using fetch array and a while cycle.<br><br>So, using fetch-all, for a normal code that returns a moderate amount of information is :<br>a) cleaner (a single line of code)<br>b) uses less memory (about 0.01% less)<br>c) faster.<br><br>php 5.6 32bits, windows 8.1 64bits  

---

If you really need this function, you can just extend the mysqli_result class with a function like this one.<br><br>

```
<?php
        public function fetch_all($resulttype = MYSQLI_NUM)
        {
            if (method_exists('mysqli_result', 'fetch_all')) # Compatibility layer with PHP < 5.3
                $res = parent::fetch_all($resulttype);
            else
                for ($res = array(); $tmp = $this->fetch_array($resulttype);) $res[] = $tmp;

            return $res;
        }
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/mysqli-result.fetch-all.php)

**[To root](/README.md)**