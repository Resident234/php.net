# explode



Here is my approach to have exploded output with multiple delimiter. <br><br>

```
<?php

//$delimiters has to be array
//$string has to be array

function multiexplode ($delimiters,$string) {
    
    $ready = str_replace($delimiters, $delimiters[0], $string);
    $launch = explode($delimiters[0], $ready);
    return  $launch;
}

$text = "here is a sample: this text, and this will be exploded. this also | this one too :)";
$exploded = multiexplode(array(",",".","|",":"),$text);

print_r($exploded);

//And output will be like this:
// Array
// (
//    [0] =&gt; here is a sample
//    [1] =&gt;  this text
//    [2] =&gt;  and this will be exploded
//    [3] =&gt;  this also 
//    [4] =&gt;  this one too 
//    [5] =&gt; )
// )

?>
```
  

#

Beaware splitting empty strings.<br><br>

```
<?php
$str = "";
$res = explode(",", $str);
print_r($res);
?>
```


If you split an empty string, you get back a one-element array with 0 as the key and an empty string for the value.

Array
(
    [0] =&gt; 
)

To solve this, just use array_filter() without callback. Quoting manual page "If the callback function is not supplied, array_filter() will remove all the entries of input that are equal to FALSE.".



```
<?php
$str = "";
$res = array_filter(explode(",", $str));
print_r($res);
?>
```
<br><br>Array<br>(<br>)  

#

The comments to use array_filter() without a callback to remove empty strings from explode&apos;s results miss the fact that array_filter will remove all elements that, to quote the manual,  "are equal to FALSE".<br><br>This includes, in particular, the string "0", which is NOT an empty string.<br><br>If you really want to filter out empty strings, use the defining feature of the empty string that it is the only string that has a length of 0. So:<br>

```
<?php
array_filter(explode(&apos;:&apos;, "1:2::3:0:4"), &apos;strlen&apos;);
?>
```
  

#

a simple one line method to explode &amp; trim whitespaces from the exploded elements<br><br>array_map(&apos;trim&apos;,explode(",",$str));<br><br>example:<br><br>$str="one  ,two  ,       three  ,  four    "; <br>print_r(array_map(&apos;trim&apos;,explode(",",$str)));<br><br>Output:<br><br>Array ( [0] =&gt; one [1] =&gt; two [2] =&gt; three [3] =&gt; four )  

#



```
<?php
// converts pure string into a trimmed keyed array
function string2KeyedArray($string, $delimiter = &apos;,&apos;, $kv = &apos;=&gt;&apos;) {
  if ($a = explode($delimiter, $string)) { // create parts
    foreach ($a as $s) { // each part
      if ($s) {
        if ($pos = strpos($s, $kv)) { // key/value delimiter
          $ka[trim(substr($s, 0, $pos))] = trim(substr($s, $pos + strlen($kv)));
        } else { // key delimiter not found
          $ka[] = trim($s);
        }
      }
    }
    return $ka;
  }
} // string2KeyedArray

$string = &apos;a=&gt;1, b=&gt;23   , $a, c=&gt; 45% , true,d =&gt; ab c &apos;;
print_r(string2KeyedArray($string));
?>
```
<br><br>Array<br>(<br>  [a] =&gt; 1<br>  [b] =&gt; 23<br>  [0] =&gt; $a<br>  [c] =&gt; 45%<br>  [1] =&gt; true<br>  [d] =&gt; ab c<br>)  

#

Here&apos;s a function for "multi" exploding a string.<br><br>

```
<?php
//the function
//Param 1 has to be an Array
//Param 2 has to be a String
function multiexplode ($delimiters,$string) {
    $ary = explode($delimiters[0],$string);
    array_shift($delimiters);
    if($delimiters != NULL) {
        foreach($ary as $key =&gt; $val) {
             $ary[$key] = multiexplode($delimiters, $val);
        }
    }
    return  $ary;
}

// Example of use
$string = "1-2-3|4-5|6:7-8-9-0|1,2:3-4|5";
$delimiters = Array(",",":","|","-");

$res = multiexplode($delimiters,$string);
echo &apos;&lt;pre&gt;&apos;;
print_r($res);
echo &apos;&lt;/pre&gt;&apos;;

//returns
/*
Array
(
    [0] =&gt; Array
        (
            [0] =&gt; Array
                (
                    [0] =&gt; Array
                        (
                            [0] =&gt; 1
                            [1] =&gt; 2
                            [2] =&gt; 3
                        )

                    [1] =&gt; Array
                        (
                            [0] =&gt; 4
                            [1] =&gt; 5
                        )

                    [2] =&gt; Array
                        (
                            [0] =&gt; 6
                        )

                )

            [1] =&gt; Array
                (
                    [0] =&gt; Array
                        (
                            [0] =&gt; 7
                            [1] =&gt; 8
                            [2] =&gt; 9
                            [3] =&gt; 0
                        )

                    [1] =&gt; Array
                        (
                            [0] =&gt; 1
                        )

                )

        )

    [1] =&gt; Array
        (
            [0] =&gt; Array
                (
                    [0] =&gt; Array
                        (
                            [0] =&gt; 2
                        )

                )

            [1] =&gt; Array
                (
                    [0] =&gt; Array
                        (
                            [0] =&gt; 3
                            [1] =&gt; 4
                        )

                    [1] =&gt; Array
                        (
                            [0] =&gt; 5
                        )

                )

        )

)
*/
?>
```
  

#

Explode does not parse a string by delimiters, in the sense that we expect to find tokens between a starting and ending delimiter, but instead splits a string into parts by using a string as the boundary of each part. Once that boundary is discovered the string is split. Whether or not that boundary is proceeded or superseded by any data is irrelevant since the parts are determined at the point a boundary is discovered. <br><br>For example: <br><br>

```
<?php 

var_dump(explode("/","/")); 

/* 
   Outputs 

   array(2) { 
     [0]=&gt; 
     string(0) "" 
     [1]=&gt; 
     string(0) "" 
   } 
*/ 

?>
```
 

The reason we have two empty strings here is that a boundary is discovered before any data has been collected from the string. The boundary splits the string into two parts even though those parts are empty. 

One way to avoid getting back empty parts (if you don&apos;t care for those empty parts) is to use array_filter on the result. 



```
<?php 

var_dump(array_filter(explode("/","/"))); 

/* 
   Outputs 

   array(0) { 
   } 
*/ 
?>
```
 <br><br>*[This note was edited by googleguy at php dot net for clarity]*  

#

[Official documentation page](https://www.php.net/manual/en/function.explode.php)

**[To root](/README.md)**