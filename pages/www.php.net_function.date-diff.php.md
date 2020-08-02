# date_diff



Powerful Function to get two date difference.<br><br>//////////////////////////////////////////////////////////////////////<br>//PARA: Date Should In YYYY-MM-DD Format<br>//RESULT FORMAT:<br>// &apos;%y Year %m Month %d Day %h Hours %i Minute %s Seconds&apos;        =&gt;  1 Year 3 Month 14 Day 11 Hours 49 Minute 36 Seconds<br>// &apos;%y Year %m Month %d Day&apos;                                    =&gt;  1 Year 3 Month 14 Days<br>// &apos;%m Month %d Day&apos;                                            =&gt;  3 Month 14 Day<br>// &apos;%d Day %h Hours&apos;                                            =&gt;  14 Day 11 Hours<br>// &apos;%d Day&apos;                                                        =&gt;  14 Days<br>// &apos;%h Hours %i Minute %s Seconds&apos;                                =&gt;  11 Hours 49 Minute 36 Seconds<br>// &apos;%i Minute %s Seconds&apos;                                        =&gt;  49 Minute 36 Seconds<br>// &apos;%h Hours                                                    =&gt;  11 Hours<br>// &apos;%a Days                                                        =&gt;  468 Days<br>//////////////////////////////////////////////////////////////////////<br>function dateDifference($date_1 , $date_2 , $differenceFormat = &apos;%a&apos; )<br>{<br>    $datetime1 = date_create($date_1);<br>    $datetime2 = date_create($date_2);<br>    <br>    $interval = date_diff($datetime1, $datetime2);<br>    <br>    return $interval-&gt;format($differenceFormat);<br>    <br>}  

---



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

       if( is_string( $dt_menor)) $dt_menor = date_create( $dt_menor);
       if( is_string( $dt_maior)) $dt_maior = date_create( $dt_maior);

       $diff = date_diff( $dt_menor, $dt_maior, ! $relative);
       
       switch( $str_interval){
           case "y": 
               $total = $diff->y + $diff->m / 12 + $diff->d / 365.25; break;
           case "m":
               $total= $diff->y * 12 + $diff->m + $diff->d/30 + $diff->h / 24;
               break;
           case "d":
               $total = $diff->y * 365.25 + $diff->m * 30 + $diff->d + $diff->h/24 + $diff->i / 60;
               break;
           case "h": 
               $total = ($diff->y * 365.25 + $diff->m * 30 + $diff->d) * 24 + $diff->h + $diff->i/60;
               break;
           case "i": 
               $total = (($diff->y * 365.25 + $diff->m * 30 + $diff->d) * 24 + $diff->h) * 60 + $diff->i + $diff->s/60;
               break;
           case "s": 
               $total = ((($diff->y * 365.25 + $diff->m * 30 + $diff->d) * 24 + $diff->h) * 60 + $diff->i)*60 + $diff->s;
               break;
          }
       if( $diff->invert)
               return -1 * $total;
       else    return $total;
   }

/* Enjoy and feedback me ;-) */
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.date-diff.php)

**[To root](/README.md)**