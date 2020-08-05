# is_nan



nan/"not a number" is not meant to see if the data type is numeric/textual/etc..<br><br>NaN is actually a set of values which can be stored in floating-point variables, but dont actually evaluate to a proper floating point number.<br><br>The floating point system has three sections: 1 bit for the sign (+/-), an 8 bit exponent, and a 23 bit fractional part.<br>There are rules governing which combinations of values can be placed into each section, and some values are reserved for numbers such as infinity. This leads to certain combinations being invalid, or in other words, not a number.  

---

[Official documentation page](https://www.php.net/manual/en/function.is-nan.php)

**[To root](/README.md)**