# array_diff_key




<div class="phpcode"><span class="html">
To return the unique elements (those with a key that exists only once in either array but not in both) try:<br>function array_unique_diff ($array1, $array2)<br>{<br>&#xA0; array_merge(array_diff_key($array1, $array2), array_diff_key($array2, $array1));<br>}<br><br>Example:<br>$array1 = array(&apos;blue&apos;&#xA0; =&gt; 1, &apos;red&apos;&#xA0; =&gt; 2, &apos;green&apos;&#xA0; =&gt; 3, &apos;purple&apos; =&gt; 4);<br>$array2 = array(&apos;green&apos; =&gt; 5, &apos;blue&apos; =&gt; 6, &apos;yellow&apos; =&gt; 7, &apos;cyan&apos;&#xA0;&#xA0; =&gt; 8);<br><br>&#xA0; array_diff_key($array1, $array2)<br><br>returns<br><br>&#xA0; array ( &apos;red&apos; =&gt; 2, &apos;purple&apos; =&gt; 4 ) <br><br>&#xA0; array_diff_key($array2, $array1)<br><br>returns<br><br>&#xA0; array ( &apos;yellow&apos; =&gt; 7, &apos;cyan&apos; =&gt; 8, )<br><br>&#xA0; array_unique_diff($array1, $array2);<br>&#xA0; <br>returns<br><br>&#xA0; array ( &apos;red&apos; =&gt; 2, &apos;purple&apos; =&gt; 4, &apos;yellow&apos; =&gt; 7, &apos;cyan&apos; =&gt; 8, )</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-diff-key.php)

**[To root](/README.md)**