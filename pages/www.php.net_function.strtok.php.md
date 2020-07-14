# strtok





```
<?php
// strtok example
$str = &apos;Hello to all of Ukraine&apos;;
echo strtok($str, &apos; &apos;).&apos; &apos;.strtok(&apos; &apos;).&apos; &apos;.strtok(&apos; &apos;);
?>
```
<br>Result:<br>Hello to all  

#

&lt;pre&gt;

```
<?php
/** get leading, trailing, and embedded separator tokens that were &apos;skipped&apos;
if for some ungodly reason you are using php to implement a simple parser that 
needs to detect nested clauses as it builds a parse tree */

$str = "(((alpha(beta))(gamma))";

$seps = &apos;()&apos;;
$tok = strtok( $str,$seps ); // return false on empty string or null
$cur = 0;      
$dumbDone = FALSE;
$done = (FALSE===$tok);
while (!$done) {
   // process skipped tokens (if any at first iteration) (special for last)
   $posTok = $dumbDone ? strlen($str) : strpos($str, $tok, $cur );
   $skippedMany = substr( $str, $cur, $posTok-$cur ); // false when 0 width
   $lenSkipped = strlen($skippedMany); // 0 when false
   if (0!==$lenSkipped) {
      $last = strlen($skippedMany) -1;
      for($i=0; $i&lt;=$last; $i++){
         $skipped = $skippedMany[$i];
         $cur += strlen($skipped);
         echo "skipped: $skipped\n";
      }
   }
   if ($dumbDone) break; // this is the only place the loop is terminated

   // process current tok
   echo "curr tok: ".$tok."\n";

   // update cursor
   $cur += strlen($tok);

   // get any next tok
   if (!$dumbDone){
      $tok = strtok($seps);
      $dumbDone = (FALSE===$tok); 
      // you&apos;re not really done till you check for trailing skipped
   }
};
?>
```
&lt;/pre&gt;  

#

[Official documentation page](https://www.php.net/manual/en/function.strtok.php)

**[To root](/README.md)**