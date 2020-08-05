# checkdnsrr



criffoh at gmail dot com is right. Before you check domain, you must convert to ascii with idn_to_ascii function:<br>http://us2.php.net/manual/en/function.idn-to-ascii.php .<br><br>var_dump(checkdnsrr(&apos;&#xF1;andu.cl&apos;, &apos;A&apos;)); // returns false<br>var_dump(checkdnsrr(idn_to_ascii(&apos;&#xF1;andu.cl&apos;), &apos;A&apos;)); // return true  

---

[Official documentation page](https://www.php.net/manual/en/function.checkdnsrr.php)

**[To root](/README.md)**