# Incrementing/Decrementing Operators




<div class="phpcode"><span class="html">
Note that <br><br>$a=&quot;9D9&quot;; var_dump(++$a);&#xA0;&#xA0; =&gt; string(3) &quot;9E0&quot;<br><br>but counting onwards from there <br><br>$a=&quot;9E0&quot;; var_dump(++$a);&#xA0;&#xA0; =&gt; float(10)<br><br>this is due to &quot;9E0&quot; being interpreted as a string representation of the float constant 9E0 (or 9e0), and thus evalutes to 9 * 10^0 = 9 (in a float context)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.increment.php)

**[To root](/)**