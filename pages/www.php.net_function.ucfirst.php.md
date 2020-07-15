# ucfirst



Simple multi-bytes ucfirst():<br><br>

```
<?php
function my_mb_ucfirst($str) {
    $fc = mb_strtoupper(mb_substr($str, 0, 1));
    return $fc.mb_substr($str, 1);
}
?>
```
  

#

A proper Turkish solution;<br><br>

```
<?php
function ucfirst_turkish($str) {
    $tmp = preg_split("//u", $str, 2, PREG_SPLIT_NO_EMPTY);
    return mb_convert_case(
        str_replace("i", "&#x130;", $tmp[0]), MB_CASE_TITLE, "UTF-8").
        $tmp[1];
}

$str = "iyilik g&#xFC;zelL&#x130;K";
echo ucfirst($str) ."\n";   // Iyilik g&#xFC;zelL&#x130;K
echo ucfirst_turkish($str); // &#x130;yilik g&#xFC;zelL&#x130;K
?>
```
  

#

I believe that mb_ucfirst will be soon added in PHP, but for now this could be useful<br>

```
<?php

if (!function_exists('mb_ucfirst') &amp;&amp; function_exists('mb_substr')) {
    function mb_ucfirst($string) {
        $string = mb_strtoupper(mb_substr($string, 0, 1)) . mb_substr($string, 1);
        return $string;
    }
}

?>
```
<br><br>it also check is mb support enabled or not  

#

Here&apos;s a function to capitalize segments of a name, and put the rest into lower case. You can pass the characters you want to use as delimiters.<br><br>i.e. 

```
<?php echo nameize("john o'grady-smith"); ?>
```


returns John O'Grady-Smith



```
<?php

function nameize($str,$a_char = array("'","-"," ")){    
    //$str contains the complete raw name string
    //$a_char is an array containing the characters we use as separators for capitalization. If you don't pass anything, there are three in there as default.
    $string = strtolower($str);
    foreach ($a_char as $temp){
        $pos = strpos($string,$temp);
        if ($pos){
            //we are in the loop because we found one of the special characters in the array, so lets split it up into chunks and capitalize each one.
            $mend = '';
            $a_split = explode($temp,$string);
            foreach ($a_split as $temp2){
                //capitalize each portion of the string which was separated at a special character
                $mend .= ucfirst($temp2).$temp;
                }
            $string = substr($mend,0,-1);
            }    
        }
    return ucfirst($string);
    }

?>
```
  

#

This is what I use for converting strings to sentence case:<br><br>

```
<?php
function sentence_case($string) {
    $sentences = preg_split('/([.?!]+)/', $string, -1, PREG_SPLIT_NO_EMPTY|PREG_SPLIT_DELIM_CAPTURE);
    $new_string = '';
    foreach ($sentences as $key => $sentence) {
        $new_string .= ($key &amp; 1) == 0?
            ucfirst(strtolower(trim($sentence))) :
            $sentence.' ';
    }
    return trim($new_string);
}

print sentence_case('HMM. WOW! WHAT?');

// Outputs: "Hmm. Wow! What?"
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.ucfirst.php)

**[To root](/README.md)**