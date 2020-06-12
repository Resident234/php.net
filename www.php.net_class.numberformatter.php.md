# The NumberFormatter class




<div class="phpcode"><span class="html">
this class seems to be painful: it is not, formatting and parsing are highly customizable, but what you probably need is really simple:
<br>
<br>if you want to localize a number use:
<br>
<br><span class="default">&lt;?php
<br>$a </span><span class="keyword">= new \</span><span class="default">NumberFormatter</span><span class="keyword">(</span><span class="string">&quot;it-IT&quot;</span><span class="keyword">, \</span><span class="default">NumberFormatter</span><span class="keyword">::</span><span class="default">DECIMAL</span><span class="keyword">);
<br>echo </span><span class="default">$a</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="default">12345.12345</span><span class="keyword">) . </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">; </span><span class="comment">// outputs 12.345,12
<br></span><span class="default">$a</span><span class="keyword">-&gt;</span><span class="default">setAttribute</span><span class="keyword">(\</span><span class="default">NumberFormatter</span><span class="keyword">::</span><span class="default">MIN_FRACTION_DIGITS</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">);
<br></span><span class="default">$a</span><span class="keyword">-&gt;</span><span class="default">setAttribute</span><span class="keyword">(\</span><span class="default">NumberFormatter</span><span class="keyword">::</span><span class="default">MAX_FRACTION_DIGITS</span><span class="keyword">, </span><span class="default">100</span><span class="keyword">); </span><span class="comment">// by default some locales got max 2 fraction digits, that is probably not what you want
<br></span><span class="keyword">echo </span><span class="default">$a</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="default">12345.12345</span><span class="keyword">) . </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">; </span><span class="comment">// outputs 12.345,12345
<br></span><span class="default">?&gt;
<br></span>
<br>if you want to print money use:
<br>
<br><span class="default">&lt;?php
<br>$a </span><span class="keyword">= new \</span><span class="default">NumberFormatter</span><span class="keyword">(</span><span class="string">&quot;it-IT&quot;</span><span class="keyword">, \</span><span class="default">NumberFormatter</span><span class="keyword">::</span><span class="default">CURRENCY</span><span class="keyword">);
<br>echo </span><span class="default">$a</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="default">12345.12345</span><span class="keyword">) . </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">; </span><span class="comment">// outputs &#x20AC;12.345,12
<br></span><span class="default">?&gt;
<br></span>
<br>if you have money data stored as (for example) US dollars and you want to print them using the it-IT notation, you need to use
<br>
<br><span class="default">&lt;?php
<br>$a </span><span class="keyword">= new \</span><span class="default">NumberFormatter</span><span class="keyword">(</span><span class="string">&quot;it-IT&quot;</span><span class="keyword">, \</span><span class="default">NumberFormatter</span><span class="keyword">::</span><span class="default">CURRENCY</span><span class="keyword">);
<br>echo </span><span class="default">$a</span><span class="keyword">-&gt;</span><span class="default">formatCurrency</span><span class="keyword">(</span><span class="default">12345</span><span class="keyword">, </span><span class="string">&quot;USD&quot;</span><span class="keyword">) . </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">; </span><span class="comment">// outputs $ 12.345,00 and it is formatted using the italian notation (comma as decimal separator)
<br></span><span class="default">?&gt;
<br></span>
<br>another useful example about currency (how to obtain the currency name by a locale string):
<br>
<br><span class="default">&lt;?php
<br>$frontEndFormatter </span><span class="keyword">= new \</span><span class="default">NumberFormatter</span><span class="keyword">(</span><span class="string">&quot;it-IT&quot;</span><span class="keyword">, \</span><span class="default">NumberFormatter</span><span class="keyword">::</span><span class="default">CURRENCY</span><span class="keyword">);
<br></span><span class="default">$adminFormatter </span><span class="keyword">= new \</span><span class="default">NumberFormatter</span><span class="keyword">(</span><span class="string">&quot;en-US&quot;</span><span class="keyword">, \</span><span class="default">NumberFormatter</span><span class="keyword">::</span><span class="default">CURRENCY</span><span class="keyword">);
<br></span><span class="default">$symbol </span><span class="keyword">= </span><span class="default">$adminFormatter</span><span class="keyword">-&gt;</span><span class="default">getSymbol</span><span class="keyword">(\</span><span class="default">NumberFormatter</span><span class="keyword">::</span><span class="default">INTL_CURRENCY_SYMBOL</span><span class="keyword">); </span><span class="comment">// got USD
<br></span><span class="keyword">echo </span><span class="default">$frontEndFormatter</span><span class="keyword">-&gt;</span><span class="default">formatCurrency</span><span class="keyword">(</span><span class="default">12345.12345</span><span class="keyword">,&#xA0; </span><span class="default">$symbol</span><span class="keyword">) . </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.numberformatter.php)

**[⬆ to root](/)**