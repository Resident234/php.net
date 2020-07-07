# Predefined Constants





I just learnt of INF today and found out that it can be used in comparisons:

&#xA0; &#xA0; echo 5000 &lt; INF ? &apos;yes&apos; : &apos;no&apos;;&#xA0; &#xA0; &#xA0;&#xA0; // outputs &apos;yes&apos;
&#xA0; &#xA0; echo INF &lt; INF ? &apos;yes&apos; : &apos;no&apos;;&#xA0; &#xA0; &#xA0; &#xA0; // outputs &apos;no&apos;
&#xA0; &#xA0; echo INF &lt;= INF ? &apos;yes&apos; : &apos;no&apos;;&#xA0; &#xA0; &#xA0;&#xA0; // outputs &apos;yes&apos;
&#xA0; &#xA0; echo INF == INF ? &apos;yes&apos; : &apos;no&apos;;&#xA0; &#xA0; &#xA0;&#xA0; // outputs &apos;yes&apos;

You can also take its negative:

&#xA0; &#xA0; echo -INF &lt; -5000 ? &apos;yes&apos; : &apos;no&apos;;&#xA0; &#xA0; // outputs &apos;yes&apos;

Division by INF is allowed:

&#xA0; &#xA0; echo 1/INF;&#xA0; &#xA0; // outputs &apos;0&apos;

  

#



There are also the predefined PHP_INT_MAX and PHP_INT_SIZE constants, that describe the range of possible integer values.

  

#

[Official documentation page](https://www.php.net/manual/en/math.constants.php)

**[To root](/README.md)**