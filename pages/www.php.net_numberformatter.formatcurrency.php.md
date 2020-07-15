# NumberFormatter::formatCurrency



While this function accepts floats for currency (in order to display cents), you should (for applications where this is critical) never store or handle money using floats, as rounding errors may occur. Work with integers (or a BigInt class if integers aren&apos;t large enough) internally instead, where the integer represents the total number of cents. An alternative (especially if you need more precision than cents) is using the BC (Binary Calculator) Math module, that handles arbitrary precision numbers with 100% accuracy.  

#

When you want to format currency&apos;s without sub units and the currency is not the one used by the given locale you need to set the currency code before as TextAttribute _BEFORE_ setting the NumberFormatter::FRACTION_DIGITS<br><br>

```
<?php
$fmt = new NumberFormatter('en_US', NumberFormatter::CURRENCY);
$fmt->setTextAttribute(NumberFormatter::CURRENCY_CODE, 'EUR');
$fmt->setAttribute(NumberFormatter::FRACTION_DIGITS, 0);
$fmt->formatCurrency(100, 'EUR');
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/numberformatter.formatcurrency.php)

**[To root](/README.md)**