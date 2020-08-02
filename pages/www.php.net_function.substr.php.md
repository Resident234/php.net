# substr



For getting a substring of UTF-8 characters, I highly recommend mb_substr<br><br>

```
<?php
        $utf8string = "cake&#xE6;&#xF8;&#xE5;";

        echo substr($utf8string,0,5);
        // output cake#
        echo mb_substr($utf8string,0,5,'UTF-8');
        //output cake&#xE6;
?>
```
  

---

may be by following functions will be easier to extract the needed sub parts from a string:<br><br>

```
<?php
after ('@', 'biohazard@online.ge');
//returns 'online.ge'
//from the first occurrence of '@'

before ('@', 'biohazard@online.ge');
//returns 'biohazard'
//from the first occurrence of '@'

between ('@', '.', 'biohazard@online.ge');
//returns 'online'
//from the first occurrence of '@'

after_last ('[', 'sin[90]*cos[180]');
//returns '180]'
//from the last occurrence of '['

before_last ('[', 'sin[90]*cos[180]');
//returns 'sin[90]*cos['
//from the last occurrence of '['

between_last ('[', ']', 'sin[90]*cos[180]');
//returns '180'
//from the last occurrence of '['
?>
```


here comes the source:



```
<?php

    function after ($this, $inthat)
    {
        if (!is_bool(strpos($inthat, $this)))
        return substr($inthat, strpos($inthat,$this)+strlen($this));
    };

    function after_last ($this, $inthat)
    {
        if (!is_bool(strrevpos($inthat, $this)))
        return substr($inthat, strrevpos($inthat, $this)+strlen($this));
    };

    function before ($this, $inthat)
    {
        return substr($inthat, 0, strpos($inthat, $this));
    };

    function before_last ($this, $inthat)
    {
        return substr($inthat, 0, strrevpos($inthat, $this));
    };

    function between ($this, $that, $inthat)
    {
        return before ($that, after($this, $inthat));
    };

    function between_last ($this, $that, $inthat)
    {
     return after_last($this, before_last($that, $inthat));
    };

// use strrevpos function in case your php version does not include it
function strrevpos($instr, $needle)
{
    $rev_pos = strpos (strrev($instr), strrev($needle));
    if ($rev_pos===false) return false;
    else return strlen($instr) - $rev_pos - strlen($needle);
};
?>
```
  

---



```
<?phpPhp 

### SUB STRING  BY WORD USING substr() and strpos()  #####

### THIS SCRIPT WILL RETURN PART OF STRING  WITHOUT WORD BREAK ###

$description = &#x2018;your description here your description here your description here your description here your description here your description here your description hereyour description here your description here&#x2019;  // your description here .

$no_letter = 30 ;

if(strlen($desctiption) > 30 )
{
     echo substr($description,0,strpos($description,&#x2019; &#x2018;,30));             //strpos to find &#x2018; &#x2018; after 30 characters.
}
else {
     echo $description;
}

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.substr.php)

**[To root](/README.md)**