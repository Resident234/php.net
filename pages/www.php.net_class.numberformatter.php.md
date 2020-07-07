# The NumberFormatter class





this class seems to be painful: it is not, formatting and parsing are highly customizable, but what you probably need is really simple:



if you want to localize a number use:





```
<?php

$a = new \NumberFormatter(&quot;it-IT&quot;, \NumberFormatter::DECIMAL);

echo $a-&gt;format(12345.12345) . &quot;&lt;br&gt;&quot;; // outputs 12.345,12

$a-&gt;setAttribute(\NumberFormatter::MIN_FRACTION_DIGITS, 0);

$a-&gt;setAttribute(\NumberFormatter::MAX_FRACTION_DIGITS, 100); // by default some locales got max 2 fraction digits, that is probably not what you want

echo $a-&gt;format(12345.12345) . &quot;&lt;br&gt;&quot;; // outputs 12.345,12345

?>
```




if you want to print money use:





```
<?php

$a = new \NumberFormatter(&quot;it-IT&quot;, \NumberFormatter::CURRENCY);

echo $a-&gt;format(12345.12345) . &quot;&lt;br&gt;&quot;; // outputs &#x20AC;12.345,12

?>
```




if you have money data stored as (for example) US dollars and you want to print them using the it-IT notation, you need to use





```
<?php

$a = new \NumberFormatter(&quot;it-IT&quot;, \NumberFormatter::CURRENCY);

echo $a-&gt;formatCurrency(12345, &quot;USD&quot;) . &quot;&lt;br&gt;&quot;; // outputs $ 12.345,00 and it is formatted using the italian notation (comma as decimal separator)

?>
```




another useful example about currency (how to obtain the currency name by a locale string):





```
<?php

$frontEndFormatter = new \NumberFormatter(&quot;it-IT&quot;, \NumberFormatter::CURRENCY);

$adminFormatter = new \NumberFormatter(&quot;en-US&quot;, \NumberFormatter::CURRENCY);

$symbol = $adminFormatter-&gt;getSymbol(\NumberFormatter::INTL_CURRENCY_SYMBOL); // got USD

echo $frontEndFormatter-&gt;formatCurrency(12345.12345,&#xA0; $symbol) . &quot;&lt;br&gt;&quot;;

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/class.numberformatter.php)

**[To root](/README.md)**