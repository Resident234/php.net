# trader_linearreg_slope





// If you don&apos;t have the php_trader library or needs more than 3 precision digits,
// you can use this function:
public static function linearreg_slope( $valuesIn, $period )
{
&#xA0; $valuesOut = array();
&#xA0; 
&#xA0; $startIdx = 0;
&#xA0; $endIdx = count($valuesIn) - 1;
&#xA0; 
&#xA0; $sumX = $period * ( $period - 1 ) * 0.5;
&#xA0; $sumXSqr = $period * ( $period - 1 ) * ( 2 * $period - 1 ) / 6;
&#xA0; $divisor = $sumX * $sumX - $period * $sumXSqr;
&#xA0; 
&#xA0; for ( $today = $startIdx, $outIdx = 0; $today &lt;= $endIdx; $today++, $outIdx++ )
&#xA0; {
&#xA0; &#xA0; $sumXY = 0;
&#xA0; &#xA0; $sumY = 0;
&#xA0; &#xA0; if ( $today &gt;= $period - 1 ) {
&#xA0; &#xA0; &#xA0; for( $aux = $period; $aux-- != 0; )
&#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $sumY += $tempValue = $valuesIn[$today - $aux];
&#xA0; &#xA0; &#xA0; &#xA0; $sumXY += (double)$aux * $tempValue;
&#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; $valuesOut[$outIdx] = ( $period * $sumXY - $sumX * $sumY) / $divisor;
&#xA0; &#xA0; }
&#xA0; }
&#xA0; 
&#xA0; return $valuesOut;
}

  

#

[Official documentation page](https://www.php.net/manual/en/function.trader-linearreg-slope.php)

**[To root](/README.md)**