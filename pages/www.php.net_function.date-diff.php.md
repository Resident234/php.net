# date_diff





Powerful Function to get two date difference.

//////////////////////////////////////////////////////////////////////
//PARA: Date Should In YYYY-MM-DD Format
//RESULT FORMAT:
// &apos;%y Year %m Month %d Day %h Hours %i Minute %s Seconds&apos;&#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; 1 Year 3 Month 14 Day 11 Hours 49 Minute 36 Seconds
// &apos;%y Year %m Month %d Day&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; 1 Year 3 Month 14 Days
// &apos;%m Month %d Day&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; 3 Month 14 Day
// &apos;%d Day %h Hours&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; 14 Day 11 Hours
// &apos;%d Day&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; 14 Days
// &apos;%h Hours %i Minute %s Seconds&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; 11 Hours 49 Minute 36 Seconds
// &apos;%i Minute %s Seconds&apos;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; 49 Minute 36 Seconds
// &apos;%h Hours&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; 11 Hours
// &apos;%a Days&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; =&gt;&#xA0; 468 Days
//////////////////////////////////////////////////////////////////////
function dateDifference($date_1 , $date_2 , $differenceFormat = &apos;%a&apos; )
{
&#xA0; &#xA0; $datetime1 = date_create($date_1);
&#xA0; &#xA0; $datetime2 = date_create($date_2);
&#xA0; &#xA0; 
&#xA0; &#xA0; $interval = date_diff($datetime1, $datetime2);
&#xA0; &#xA0; 
&#xA0; &#xA0; return $interval-&gt;format($differenceFormat);
&#xA0; &#xA0; 
}

  

#





```
<?php 
/* 
 * A mathematical decimal difference between two informed dates 
 *
 * Author: Sergio Abreu
 * Website: http://sites.sitesbr.net
 *
 * Features: 
 * Automatic conversion on dates informed as string.
 * Possibility of absolute values (always +) or relative (-/+)
*/

function s_datediff( $str_interval, $dt_menor, $dt_maior, $relative=false){

&#xA0; &#xA0; &#xA0;&#xA0; if( is_string( $dt_menor)) $dt_menor = date_create( $dt_menor);
&#xA0; &#xA0; &#xA0;&#xA0; if( is_string( $dt_maior)) $dt_maior = date_create( $dt_maior);

&#xA0; &#xA0; &#xA0;&#xA0; $diff = date_diff( $dt_menor, $dt_maior, ! $relative);
&#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0;&#xA0; switch( $str_interval){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; case &quot;y&quot;: 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $total = $diff-&gt;y + $diff-&gt;m / 12 + $diff-&gt;d / 365.25; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; case &quot;m&quot;:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $total= $diff-&gt;y * 12 + $diff-&gt;m + $diff-&gt;d/30 + $diff-&gt;h / 24;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; case &quot;d&quot;:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $total = $diff-&gt;y * 365.25 + $diff-&gt;m * 30 + $diff-&gt;d + $diff-&gt;h/24 + $diff-&gt;i / 60;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; case &quot;h&quot;: 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $total = ($diff-&gt;y * 365.25 + $diff-&gt;m * 30 + $diff-&gt;d) * 24 + $diff-&gt;h + $diff-&gt;i/60;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; case &quot;i&quot;: 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $total = (($diff-&gt;y * 365.25 + $diff-&gt;m * 30 + $diff-&gt;d) * 24 + $diff-&gt;h) * 60 + $diff-&gt;i + $diff-&gt;s/60;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; case &quot;s&quot;: 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $total = ((($diff-&gt;y * 365.25 + $diff-&gt;m * 30 + $diff-&gt;d) * 24 + $diff-&gt;h) * 60 + $diff-&gt;i)*60 + $diff-&gt;s;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0;&#xA0; if( $diff-&gt;invert)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return -1 * $total;
&#xA0; &#xA0; &#xA0;&#xA0; else&#xA0; &#xA0; return $total;
&#xA0;&#xA0; }

/* Enjoy and feedback me ;-) */
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.date-diff.php)

**[To root](/README.md)**