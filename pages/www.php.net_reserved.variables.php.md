# Predefined Variables



Warning: $_SERVER[&apos;PHP_SELF&apos;] can include arbitrary user input. The documentation should be updated to reflect this.<br><br>The request "http://example.com/info.php/attack%20here" will run /info.php, but in Apache $_SERVER[&apos;PHP_SELF&apos;] will equal "/info.php/attack here". This is a feature, but it means that PHP_SELF must be treated as user input.<br><br>The attack string could contain urlencoded HTML and JavaScript (cross-site scripting) or it could contain urlencoded linebreaks (HTTP response-splitting).<br><br>The use of $_SERVER[&apos;SCRIPT_NAME&apos;] is recommended instead.  

---

[Official documentation page](https://www.php.net/manual/en/reserved.variables.php)

**[To root](/README.md)**