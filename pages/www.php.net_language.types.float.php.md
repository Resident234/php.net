# Floating point precision





$x = 8 - 6.4;&#xA0; // which is equal to 1.6
$y = 1.6;
var_dump($x == $y); // is not true

PHP thinks that 1.6 (coming from a difference) is not equal to 1.6. To make it work, use round()

var_dump(round($x, 2) == round($y, 2)); // this is true

This happens probably because $x is not really 1.6, but 1.599999.. and var_dump shows it to you as being 1.6.

  

#



General computing hint: If you&apos;re keeping track of money, do yourself and your users the favor of handling everything internally in cents and do as much math as you can in integers. Store values in cents if at all possible. Add and subtract in cents. At every operation that wii involve floats, ask yourself &quot;what will happen in the real world if I get a fraction of a cent here&quot; and if the answer is that this operation will generate a transaction in integer cents, do not try to carry fictional fractional accuracy that will only screw things up later.

  

#



just a comment on something the &quot;Floating point precision&quot; inset, which goes: &quot;This is related to .... 0.3333333.&quot;

While the author probably knows what they are talking about, this loss of precision has nothing to do with decimal notation, it has to do with representation as a floating-point binary in a finite register, such as while 0.8 terminates in decimal, it is the repeating 0.110011001100... in binary, which is truncated.&#xA0; 0.1 and 0.7 are also non-terminating in binary, so they are also truncated, and the sum of these truncated numbers does not add up to the truncated binary representation of 0.8 (which is why (floor)(0.8*10) yields a different, more intuitive, result).&#xA0; However, since 2 is a factor of 10, any number that terminates in binary also terminates in decimal.

  

#



I&apos;d like to point out a &quot;feature&quot; of PHP&apos;s floating point support that isn&apos;t made clear anywhere here, and was driving me insane.



This test (where var_dump says that $a=0.1 and $b=0.1)



if ($a&gt;=$b) echo &quot;blah!&quot;;



Will fail in some cases due to hidden precision (standard C problem, that PHP docs make no mention of, so I assumed they had gotten rid of it). I should point out that I originally thought this was an issue with the floats being stored as strings, so I forced them to be floats and they still didn&apos;t get evaluated properly (probably 2 different problems there).



To fix, I had to do this horrible kludge (the equivelant of anyway):



if (round($a,3)&gt;=round($b,3)) echo &quot;blah!&quot;;



THIS works. Obviously even though var_dump says the variables are identical, and they SHOULD BE identical (started at 0.01 and added 0.001 repeatedly), they&apos;re not. There&apos;s some hidden precision there that was making me tear my hair out. Perhaps this should be added to the documentation?

  

#

[Official documentation page](https://www.php.net/manual/en/language.types.float.php)

**[To root](/README.md)**