# bcadd



I made this to add an unlimited size of numbers together..<br><br>This could be useful for those without the BCMath extension.<br><br>It allows decimals, and optional $Scale parameter.  If $Scale isn&apos;t specified, then it&apos;ll automatically adjust to show the correct number of decimals.<br><br>

```
<?php

function Add($Num1,$Num2,$Scale=null) {
  // check if they're valid positive numbers, extract the whole numbers and decimals
  if(!preg_match("/^\+?(\d+)(\.\d+)?$/",$Num1,$Tmp1)||
     !preg_match("/^\+?(\d+)(\.\d+)?$/",$Num2,$Tmp2)) return('0');

  // this is where the result is stored
  $Output=array();

  // remove ending zeroes from decimals and remove point
  $Dec1=isset($Tmp1[2])?rtrim(substr($Tmp1[2],1),'0'):'';
  $Dec2=isset($Tmp2[2])?rtrim(substr($Tmp2[2],1),'0'):'';

  // calculate the longest length of decimals
  $DLen=max(strlen($Dec1),strlen($Dec2));

  // if $Scale is null, automatically set it to the amount of decimal places for accuracy
  if($Scale==null) $Scale=$DLen;

  // remove leading zeroes and reverse the whole numbers, then append padded decimals on the end
  $Num1=strrev(ltrim($Tmp1[1],'0').str_pad($Dec1,$DLen,'0'));
  $Num2=strrev(ltrim($Tmp2[1],'0').str_pad($Dec2,$DLen,'0'));

  // calculate the longest length we need to process
  $MLen=max(strlen($Num1),strlen($Num2));

  // pad the two numbers so they are of equal length (both equal to $MLen)
  $Num1=str_pad($Num1,$MLen,'0');
  $Num2=str_pad($Num2,$MLen,'0');

  // process each digit, keep the ones, carry the tens (remainders)
  for($i=0;$i&lt;$MLen;$i++) {
    $Sum=((int)$Num1{$i}+(int)$Num2{$i});
    if(isset($Output[$i])) $Sum+=$Output[$i];
    $Output[$i]=$Sum%10;
    if($Sum&gt;9) $Output[$i+1]=1;
  }

  // convert the array to string and reverse it
  $Output=strrev(implode($Output));

  // substring the decimal digits from the result, pad if necessary (if $Scale &gt; amount of actual decimals)
  // next, since actual zero values can cause a problem with the substring values, if so, just simply give '0'
  // next, append the decimal value, if $Scale is defined, and return result
  $Decimal=str_pad(substr($Output,-$DLen,$Scale),$Scale,'0');
  $Output=(($MLen-$DLen&lt;1)?'0':substr($Output,0,-$DLen));
  $Output.=(($Scale&gt;0)?".{$Decimal}":'');
  return($Output);
}

$A="5650175242.508133742";
$B="308437806.831153821478770";

printf("  Add(%s,%s);\r\n// %s\r\n\r\n",$A,$B,  Add($A,$B));
printf("BCAdd(%s,%s);\r\n// %s\r\n\r\n",$A,$B,BCAdd($A,$B));

/*
  This will produce the following..
    Add(5650175242.508133742,308437806.831153821478770);
  // 5958613049.33928756347877

  BCAdd(5650175242.508133742,308437806.831153821478770);
  // 5958613049
*/

?>
```
<br><br>It was a fun experience making, and thought I&apos;d share it.<br>Enjoy,<br>Nitrogen.  

#

[Official documentation page](https://www.php.net/manual/en/function.bcadd.php)

**[To root](/README.md)**