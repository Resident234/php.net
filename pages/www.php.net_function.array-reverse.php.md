# array_reverse



Needed to just reverse keys. Don&apos;t flog me if there is a better way. This was a simple solution.<br><br>

```
<?php
function array_reverse_keys($ar){
    return array_reverse(array_reverse($ar,true),false);
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.array-reverse.php)

**[To root](/README.md)**