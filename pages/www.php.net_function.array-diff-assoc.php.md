# array_diff_assoc



The array_diff_assoc_array from "chinello at gmail dot com" (and others) will not work for arrays with null values. That&apos;s because !isset is true when an array key doesn&apos;t exists or is set to null.<br><br>(sorry for the changed indent-style)<br>

```
<?php
function array_diff_assoc_recursive($array1, $array2) {
    $difference=array();
    foreach($array1 as $key => $value) {
        if( is_array($value) ) {
            if( !isset($array2[$key]) || !is_array($array2[$key]) ) {
                $difference[$key] = $value;
            } else {
                $new_diff = array_diff_assoc_recursive($value, $array2[$key]);
                if( !empty($new_diff) )
                    $difference[$key] = $new_diff;
            }
        } else if( !array_key_exists($key,$array2) || $array2[$key] !== $value ) {
            $difference[$key] = $value;
        }
    }
    return $difference;
}
?>
```


And here an example (note index 'b' in the output):


```
<?php
$a1=array( 'a' => 0, 'b' => null, 'c' => array( 'd' => null ) );
$a2=array( 'a' => 0, 'b' => null );

var_dump( array_diff_assoc_recursive( $a1, $a2 ) );
var_dump( chinello_array_diff_assoc_recursive( $a1, $a2 ) );
?>
```
<br>array(1) {<br>  ["c"]=&gt;<br>  array(1) {<br>    ["d"]=&gt;<br>    NULL<br>  }<br>}<br><br>array(2) {<br>  ["b"]=&gt;<br>  NULL<br>  ["c"]=&gt;<br>  array(1) {<br>    ["d"]=&gt;<br>    NULL<br>  }<br>}  

---

If you&apos;re looking for a true array_diff_assoc, comparing arrays to determine the difference between two, finding missing values from both, you can use this along with array_merge.<br><br>$a = array(&apos;a&apos;=&gt;1,&apos;b&apos;=&gt;2,&apos;c&apos;=&gt;3);<br>$b = array(&apos;a&apos;=&gt;1,&apos;b&apos;=&gt;2,&apos;d&apos;=&gt;4);<br>print_r(array_diff_assoc($a,$b));<br>// returns:<br>array<br>(<br>    [c] =&gt; 3<br>)<br><br>print_r(array_diff_assoc($b,$a));<br>// returns <br>array<br>(<br>    [d] =&gt; 4<br>)<br><br>print_r(array_merge(array_diff_assoc($a,$b),array_diff_assoc($b,$a)));<br>// returns<br>array<br>(<br>    [c] =&gt; 3<br>    [d] =&gt; 4<br>)  

---

The direction of the arguments does actually make a difference:<br><br>

```
<?php
$a = array(
    'x' => 'x',
    'y' => 'y',
    'z' => 'z',
    't' => 't',
);

$b = array(
    'x' => 'x',
    'y' => 'y',
    'z' => 'z',
    't' => 't',
    'g' => 'g',
);

print_r(array_diff_assoc($a, $b));
print_r(array_diff_assoc($b, $a));
?>
```
<br><br>echoes:<br><br>Array<br>(<br>)<br>Array<br>(<br>    [g] =&gt; g<br>)  

---

The following will recursively do an array_diff_assoc, which will calculate differences on a multi-dimensional level.  This not display any notices if a key don&apos;t exist and if error_reporting is set to E_ALL:<br><br>

```
<?php
function array_diff_assoc_recursive($array1, $array2)
{
    foreach($array1 as $key => $value)
    {
        if(is_array($value))
        {
              if(!isset($array2[$key]))
              {
                  $difference[$key] = $value;
              }
              elseif(!is_array($array2[$key]))
              {
                  $difference[$key] = $value;
              }
              else
              {
                  $new_diff = array_diff_assoc_recursive($value, $array2[$key]);
                  if($new_diff != FALSE)
                  {
                        $difference[$key] = $new_diff;
                  }
              }
          }
          elseif(!isset($array2[$key]) || $array2[$key] != $value)
          {
              $difference[$key] = $value;
          }
    }
    return !isset($difference) ? 0 : $difference;
}
?>
```
<br><br>[NOTE BY danbrown AT php DOT net: This is a combination of efforts from previous notes deleted.  Contributors included (Michael Johnson), (jochem AT iamjochem DAWT com), (sc1n AT yahoo DOT com), and (anders DOT carlsson AT mds DOT mdh DOT se).]  

---

[Official documentation page](https://www.php.net/manual/en/function.array-diff-assoc.php)

**[To root](/README.md)**