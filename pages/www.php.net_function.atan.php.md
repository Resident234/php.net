# atan




<div class="phpcode"><span class="html">
Contrary to the current description, it should hold y == tan(atan(y)) for ALL&#xA0; y.<br>However, x == atan(tan(x)) only holds for those x which are in the range of atan, which are those x with -pi/2 &lt; x &lt; pi/2.<br><br>Of course, those equalities are limited by precision. On my machine<br>tan(atan(1000)) returns 1000.0000000001.<br>atan(tan(0)) returns 0 (correct).<br>atan(tan(M_PI)) returns -1.2246467991474E-16 instead of 0.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.atan.php)

**[To root](/README.md)**