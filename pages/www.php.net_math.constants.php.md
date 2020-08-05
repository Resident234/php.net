# Predefined Constants



I just learnt of INF today and found out that it can be used in comparisons:<br><br>    echo 5000 &lt; INF ? &apos;yes&apos; : &apos;no&apos;;       // outputs &apos;yes&apos;<br>    echo INF &lt; INF ? &apos;yes&apos; : &apos;no&apos;;        // outputs &apos;no&apos;<br>    echo INF &lt;= INF ? &apos;yes&apos; : &apos;no&apos;;       // outputs &apos;yes&apos;<br>    echo INF == INF ? &apos;yes&apos; : &apos;no&apos;;       // outputs &apos;yes&apos;<br><br>You can also take its negative:<br><br>    echo -INF &lt; -5000 ? &apos;yes&apos; : &apos;no&apos;;    // outputs &apos;yes&apos;<br><br>Division by INF is allowed:<br><br>    echo 1/INF;    // outputs &apos;0&apos;  

---

There are also the predefined PHP_INT_MAX and PHP_INT_SIZE constants, that describe the range of possible integer values.  

---

[Official documentation page](https://www.php.net/manual/en/math.constants.php)

**[To root](/README.md)**