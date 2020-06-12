# bin2hex




<div class="phpcode"><span class="html">
This function is for converting binary data into a hexadecimal string representation.&#xA0; This function is not for converting strings representing binary digits into hexadecimal.&#xA0; If you want that functionality, you can simply do this:<br><br><span class="default">&lt;?php<br>$binary </span><span class="keyword">= </span><span class="string">&quot;11111001&quot;</span><span class="keyword">;<br></span><span class="default">$hex </span><span class="keyword">= </span><span class="default">dechex</span><span class="keyword">(</span><span class="default">bindec</span><span class="keyword">(</span><span class="default">$binary</span><span class="keyword">));<br>echo </span><span class="default">$hex</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>This would output &quot;f9&quot;.&#xA0; Just remember that there is a very big difference between binary data and a string representation of binary.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.bin2hex.php)

**[⬆ to root](/)**