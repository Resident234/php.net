# trader_linearreg_slope



// If you don&apos;t have the php_trader library or needs more than 3 precision digits,<br>// you can use this function:<br>public static function linearreg_slope( $valuesIn, $period )<br>{<br>  $valuesOut = array();<br>  <br>  $startIdx = 0;<br>  $endIdx = count($valuesIn) - 1;<br>  <br>  $sumX = $period * ( $period - 1 ) * 0.5;<br>  $sumXSqr = $period * ( $period - 1 ) * ( 2 * $period - 1 ) / 6;<br>  $divisor = $sumX * $sumX - $period * $sumXSqr;<br>  <br>  for ( $today = $startIdx, $outIdx = 0; $today &lt;= $endIdx; $today++, $outIdx++ )<br>  {<br>    $sumXY = 0;<br>    $sumY = 0;<br>    if ( $today &gt;= $period - 1 ) {<br>      for( $aux = $period; $aux-- != 0; )<br>      {<br>        $sumY += $tempValue = $valuesIn[$today - $aux];<br>        $sumXY += (double)$aux * $tempValue;<br>      }<br>      $valuesOut[$outIdx] = ( $period * $sumXY - $sumX * $sumY) / $divisor;<br>    }<br>  }<br>  <br>  return $valuesOut;<br>}  

---

[Official documentation page](https://www.php.net/manual/en/function.trader-linearreg-slope.php)

**[To root](/README.md)**