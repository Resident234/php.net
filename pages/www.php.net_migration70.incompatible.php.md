# Backward incompatible changes



[Editor&apos;s note: fixed limit on user request]<br><br>As a mathematician, 3/0 == +INF IS JUST WRONG. You can&apos;t just assume 3/0 == lim_{x-&gt;0+} 3/x, which is +INF indeed, because division IS NOT A CONTINUOUS FUNCTION in x == 0.<br><br>Also, 3/0 == +INF ("positive" infinity) while -3/0 == -INF ("negative" infinity) requires the assumption that 0 is a positive number, which is just as illogical as it looks like.<br><br>The fact that a warning is emitted is good, but it should definitely equals to NaN. &#xB1;INF is just illogical (and arithmetically wrong).<br><br>Except for this "detail", looks an amazing update, can&apos;t wait to test it even further!<br><br>Cheers,<br>P.  

#

Although $x/0 is technically not infinity in a purely mathematical sense, when you understand why the IEEE float includes a value for infinity, and returns infinity in this case, it makes sense that PHP would agree with this.<br><br>The reason is that programmers don&apos;t usually divide by 0 on purpose. A value of 0 as a divisor usually occurs due to underflow, i.e. a value which is too small to be represented as a float. So, for example, if you have values like:<br>$x = 1;<br>$y = 1e-15 * 1e-15;<br>$z = $x/$y;<br>Because $y will have underflowed to 0, the division operation will throw the division by zero warning, and $z will be set to INF. In a better computer, however, $y would not have the value 0 (it would be 1e-30) and $z would not have the value INF (it would be 1e30).<br><br>In other words, 0 is not only representative of an actual 0, but also a very small number which float cannot represent correctly (underflow), and INF does not only represent infinity, but also a very big number which float cannot represent correctly (overflow). We do the best we can within the limitations of floating point values, which are really just good approximations of the actual numbers being represented.<br><br>What does bother me is that division by zero is handled in two different ways depending on the operator. I would have preferred the new DivisionByZeroError exception to be thrown in all cases, for consistency and to enforce good programming practices.  

#

As a programmer, I don&apos;t care whether 3/0 is INF or NaN. Both answers are (probably) equally useless, and tell me that something somewhere else is screwed up.  

#

About the division by zero, please see discussion to IEEEE 754<br>For example: http://stackoverflow.com/questions/14682005/why-does-division-by-zero-in-ieee754-standard-results-in-infinite-value  

#

split() was also removed in 7.0, so be sure to check your old code for it as well as the functions listed in this doc  

#

[Official documentation page](https://www.php.net/manual/en/migration70.incompatible.php)

**[To root](/README.md)**