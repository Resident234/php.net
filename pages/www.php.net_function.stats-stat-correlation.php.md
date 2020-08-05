# stats_stat_correlation



undefined for me, thus I&apos;ve implemented my own correlation which is much faster and simpler than the one provided above.<br><br>function Corr($x, $y){<br><br>$length= count($x);<br>$mean1=array_sum($x) / $length;<br>$mean2=array_sum($y) / $length;<br><br>$a=0;<br>$b=0;<br>$axb=0;<br>$a2=0;<br>$b2=0;<br><br>for($i=0;$i&lt;$length;$i++)<br>{<br>$a=$x[$i]-$mean1;<br>$b=$y[$i]-$mean2;<br>$axb=$axb+($a*$b);<br>$a2=$a2+ pow($a,2);<br>$b2=$b2+ pow($b,2);<br>}<br><br>$corr= $axb / sqrt($a2*$b2);<br><br>return $corr;<br>}  

---

[Official documentation page](https://www.php.net/manual/en/function.stats-stat-correlation.php)

**[To root](/README.md)**