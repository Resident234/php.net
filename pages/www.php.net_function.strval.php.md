# strval



As of PHP 5.2, strval() will return the string value of an object, calling its __toString() method to determine what that value is.  

#

If you want to convert an integer into an English word string, eg. 29 -&gt; twenty-nine, then here&apos;s a function to do it.<br><br>Note on use of fmod()<br>  I used the floating point fmod() in preference to the % operator, because % converts the operands to int, corrupting values outside of the range [-2147483648, 2147483647]<br><br>I haven&apos;t bothered with "billion" because the word means 10e9 or 10e12 depending who you ask.<br><br>The function returns &apos;#&apos; if the argument does not represent a whole number.<br><br>

```
<?php
$nwords = array( "zero", "one", "two", "three", "four", "five", "six", "seven",
                   "eight", "nine", "ten", "eleven", "twelve", "thirteen",
                   "fourteen", "fifteen", "sixteen", "seventeen", "eighteen",
                   "nineteen", "twenty", 30 =&gt; "thirty", 40 =&gt; "forty",
                   50 =&gt; "fifty", 60 =&gt; "sixty", 70 =&gt; "seventy", 80 =&gt; "eighty",
                   90 =&gt; "ninety" );

function int_to_words($x) {
   global $nwords;

   if(!is_numeric($x))
      $w = &apos;#&apos;;
   else if(fmod($x, 1) != 0)
      $w = &apos;#&apos;;
   else {
      if($x &lt; 0) {
         $w = &apos;minus &apos;;
         $x = -$x;
      } else
         $w = &apos;&apos;;
      // ... now $x is a non-negative integer.

      if($x &lt; 21)   // 0 to 20
         $w .= $nwords[$x];
      else if($x &lt; 100) {   // 21 to 99
         $w .= $nwords[10 * floor($x/10)];
         $r = fmod($x, 10);
         if($r &gt; 0)
            $w .= &apos;-&apos;. $nwords[$r];
      } else if($x &lt; 1000) {   // 100 to 999
         $w .= $nwords[floor($x/100)] .&apos; hundred&apos;;
         $r = fmod($x, 100);
         if($r &gt; 0)
            $w .= &apos; and &apos;. int_to_words($r);
      } else if($x &lt; 1000000) {   // 1000 to 999999
         $w .= int_to_words(floor($x/1000)) .&apos; thousand&apos;;
         $r = fmod($x, 1000);
         if($r &gt; 0) {
            $w .= &apos; &apos;;
            if($r &lt; 100)
               $w .= &apos;and &apos;;
            $w .= int_to_words($r);
         }
      } else {    //  millions
         $w .= int_to_words(floor($x/1000000)) .&apos; million&apos;;
         $r = fmod($x, 1000000);
         if($r &gt; 0) {
            $w .= &apos; &apos;;
            if($r &lt; 100)
               $word .= &apos;and &apos;;
            $w .= int_to_words($r);
         }
      }
   }
   return $w;
}

?>
```


Usage:


```
<?php
echo &apos;There are currently &apos;. int_to_words($count) . &apos; members logged on.&apos;;
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.strval.php)

**[To root](/README.md)**