# trader_linearreg_slope




<div class="phpcode"><span class="html">
// If you don&apos;t have the php_trader library or needs more than 3 precision digits,<br>// you can use this function:<br>public static function linearreg_slope( $valuesIn, $period )<br>{<br>&#xA0; $valuesOut = array();<br>&#xA0; <br>&#xA0; $startIdx = 0;<br>&#xA0; $endIdx = count($valuesIn) - 1;<br>&#xA0; <br>&#xA0; $sumX = $period * ( $period - 1 ) * 0.5;<br>&#xA0; $sumXSqr = $period * ( $period - 1 ) * ( 2 * $period - 1 ) / 6;<br>&#xA0; $divisor = $sumX * $sumX - $period * $sumXSqr;<br>&#xA0; <br>&#xA0; for ( $today = $startIdx, $outIdx = 0; $today &lt;= $endIdx; $today++, $outIdx++ )<br>&#xA0; {<br>&#xA0; &#xA0; $sumXY = 0;<br>&#xA0; &#xA0; $sumY = 0;<br>&#xA0; &#xA0; if ( $today &gt;= $period - 1 ) {<br>&#xA0; &#xA0; &#xA0; for( $aux = $period; $aux-- != 0; )<br>&#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; $sumY += $tempValue = $valuesIn[$today - $aux];<br>&#xA0; &#xA0; &#xA0; &#xA0; $sumXY += (double)$aux * $tempValue;<br>&#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; $valuesOut[$outIdx] = ( $period * $sumXY - $sumX * $sumY) / $divisor;<br>&#xA0; &#xA0; }<br>&#xA0; }<br>&#xA0; <br>&#xA0; return $valuesOut;<br>}</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.trader-linearreg-slope.php)

**[To root](/README.md)**