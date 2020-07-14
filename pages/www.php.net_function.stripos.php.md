# stripos



I found myself needing to find the first position of multiple needles in one haystack.  So I wrote this little function:<br><br>

```
<?php
function multineedle_stripos($haystack, $needles, $offset=0) {
    foreach($needles as $needle) {
        $found[$needle] = stripos($haystack, $needle, $offset);
    }
    return $found;
}

// It works as such:
$haystack = "The quick brown fox jumps over the lazy dog.";
$needle = array("fox", "dog", ".", "duck")
var_dump(multineedle_stripos($haystack, $needle));
/* Output:
   array(3) {
     ["fox"]=&gt;
     int(16)
     ["dog"]=&gt;
     int(40)
     ["."]=&gt;
     int(43)
     ["duck"]=&gt;
     bool(false)
   }
*/
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.stripos.php)

**[To root](/README.md)**