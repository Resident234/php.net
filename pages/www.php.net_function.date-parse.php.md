# date_parse



A warning to others. Some keys will return with a default value where others will return as false if the date string has it omitted. Unsure if this is a bug or feature, but hopefully this will save someone some time.<br>

```
<?php
///Example
$input = "Feb 2010";
$info = date_parse($input);
var_dump($info);

/*Returns:
array(12) { 
    ["year"]=> int(2010)
    ["month"]=> int(2)
    ["day"]=> int(1)    //&lt;---expected false like below
    ["hour"]=> bool(false)
    ["minute"]=> bool(false)
    ["second"]=> bool(false)
    ["fraction"]=> bool(false)
    ["warning_count"]=> int(0)
    ["warnings"]=> array(0) { }
    ["error_count"]=> int(0)
    ["errors"]=> array(0) { }
    ["is_localtime"]=> bool(false)
}*/
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.date-parse.php)

**[To root](/README.md)**