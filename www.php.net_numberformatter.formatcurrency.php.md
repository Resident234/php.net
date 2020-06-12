# NumberFormatter::formatCurrency




<div class="phpcode"><span class="html">
While this function accepts floats for currency (in order to display cents), you should (for applications where this is critical) never store or handle money using floats, as rounding errors may occur. Work with integers (or a BigInt class if integers aren&apos;t large enough) internally instead, where the integer represents the total number of cents. An alternative (especially if you need more precision than cents) is using the BC (Binary Calculator) Math module, that handles arbitrary precision numbers with 100% accuracy.</span>
</div>
  

#


<div class="phpcode"><span class="html">
When you want to format currency&apos;s without sub units and the currency is not the one used by the given locale you need to set the currency code before as TextAttribute _BEFORE_ setting the NumberFormatter::FRACTION_DIGITS<br><br><span class="default">&lt;?php<br>$fmt </span><span class="keyword">= new </span><span class="default">NumberFormatter</span><span class="keyword">(</span><span class="string">&apos;en_US&apos;</span><span class="keyword">, </span><span class="default">NumberFormatter</span><span class="keyword">::</span><span class="default">CURRENCY</span><span class="keyword">);<br></span><span class="default">$fmt</span><span class="keyword">-&gt;</span><span class="default">setTextAttribute</span><span class="keyword">(</span><span class="default">NumberFormatter</span><span class="keyword">::</span><span class="default">CURRENCY_CODE</span><span class="keyword">, </span><span class="string">&apos;EUR&apos;</span><span class="keyword">);<br></span><span class="default">$fmt</span><span class="keyword">-&gt;</span><span class="default">setAttribute</span><span class="keyword">(</span><span class="default">NumberFormatter</span><span class="keyword">::</span><span class="default">FRACTION_DIGITS</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">);<br></span><span class="default">$fmt</span><span class="keyword">-&gt;</span><span class="default">formatCurrency</span><span class="keyword">(</span><span class="default">100</span><span class="keyword">, </span><span class="string">&apos;EUR&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/numberformatter.formatcurrency.php)

**[⬆ to root](/)**