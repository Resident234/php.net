# bcadd





I made this to add an unlimited size of numbers together..



This could be useful for those without the BCMath extension.



It allows decimals, and optional $Scale parameter.&#xA0; If $Scale isn&apos;t specified, then it&apos;ll automatically adjust to show the correct number of decimals.





```
<?php



function Add($Num1,$Num2,$Scale=null) {

&#xA0; // check if they&apos;re valid positive numbers, extract the whole numbers and decimals

&#xA0; if(!preg_match(&quot;/^\+?(\d+)(\.\d+)?$/&quot;,$Num1,$Tmp1)||

&#xA0; &#xA0;&#xA0; !preg_match(&quot;/^\+?(\d+)(\.\d+)?$/&quot;,$Num2,$Tmp2)) return(&apos;0&apos;);



&#xA0; // this is where the result is stored

&#xA0; $Output=array();



&#xA0; // remove ending zeroes from decimals and remove point

&#xA0; $Dec1=isset($Tmp1[2])?rtrim(substr($Tmp1[2],1),&apos;0&apos;):&apos;&apos;;

&#xA0; $Dec2=isset($Tmp2[2])?rtrim(substr($Tmp2[2],1),&apos;0&apos;):&apos;&apos;;



&#xA0; // calculate the longest length of decimals

&#xA0; $DLen=max(strlen($Dec1),strlen($Dec2));



&#xA0; // if $Scale is null, automatically set it to the amount of decimal places for accuracy

&#xA0; if($Scale==null) $Scale=$DLen;



&#xA0; // remove leading zeroes and reverse the whole numbers, then append padded decimals on the end

&#xA0; $Num1=strrev(ltrim($Tmp1[1],&apos;0&apos;).str_pad($Dec1,$DLen,&apos;0&apos;));

&#xA0; $Num2=strrev(ltrim($Tmp2[1],&apos;0&apos;).str_pad($Dec2,$DLen,&apos;0&apos;));



&#xA0; // calculate the longest length we need to process

&#xA0; $MLen=max(strlen($Num1),strlen($Num2));



&#xA0; // pad the two numbers so they are of equal length (both equal to $MLen)

&#xA0; $Num1=str_pad($Num1,$MLen,&apos;0&apos;);

&#xA0; $Num2=str_pad($Num2,$MLen,&apos;0&apos;);



&#xA0; // process each digit, keep the ones, carry the tens (remainders)

&#xA0; for($i=0;$i&lt;$MLen;$i++) {

&#xA0; &#xA0; $Sum=((int)$Num1{$i}+(int)$Num2{$i});

&#xA0; &#xA0; if(isset($Output[$i])) $Sum+=$Output[$i];

&#xA0; &#xA0; $Output[$i]=$Sum%10;

&#xA0; &#xA0; if($Sum&gt;9) $Output[$i+1]=1;

&#xA0; }



&#xA0; // convert the array to string and reverse it

&#xA0; $Output=strrev(implode($Output));



&#xA0; // substring the decimal digits from the result, pad if necessary (if $Scale &gt; amount of actual decimals)

&#xA0; // next, since actual zero values can cause a problem with the substring values, if so, just simply give &apos;0&apos;

&#xA0; // next, append the decimal value, if $Scale is defined, and return result

&#xA0; $Decimal=str_pad(substr($Output,-$DLen,$Scale),$Scale,&apos;0&apos;);

&#xA0; $Output=(($MLen-$DLen&lt;1)?&apos;0&apos;:substr($Output,0,-$DLen));

&#xA0; $Output.=(($Scale&gt;0)?&quot;.{$Decimal}&quot;:&apos;&apos;);

&#xA0; return($Output);

}



$A=&quot;5650175242.508133742&quot;;

$B=&quot;308437806.831153821478770&quot;;



printf(&quot;&#xA0; Add(%s,%s);\r\n// %s\r\n\r\n&quot;,$A,$B,&#xA0; Add($A,$B));

printf(&quot;BCAdd(%s,%s);\r\n// %s\r\n\r\n&quot;,$A,$B,BCAdd($A,$B));



/*

&#xA0; This will produce the following..

&#xA0; &#xA0; Add(5650175242.508133742,308437806.831153821478770);

&#xA0; // 5958613049.33928756347877



&#xA0; BCAdd(5650175242.508133742,308437806.831153821478770);

&#xA0; // 5958613049

*/



?>
```




It was a fun experience making, and thought I&apos;d share it.

Enjoy,

Nitrogen.

  

#

[Official documentation page](https://www.php.net/manual/en/function.bcadd.php)

**[To root](/README.md)**