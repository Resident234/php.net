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
    ["year"]=&gt; int(2010)
    ["month"]=&gt; int(2)
    ["day"]=&gt; int(1)    //&lt;---expected false like below
    ["hour"]=&gt; bool(false)
    ["minute"]=&gt; bool(false)
    ["second"]=&gt; bool(false)
    ["fraction"]=&gt; bool(false)
    ["warning_count"]=&gt; int(0)
    ["warnings"]=&gt; array(0) { }
    ["error_count"]=&gt; int(0)
    ["errors"]=&gt; array(0) { }
    ["is_localtime"]=&gt; bool(false)
}*/
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.date-parse.php)

**[To root](/README.md)**