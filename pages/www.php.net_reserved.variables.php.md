# Predefined Variables





Warning: $_SERVER[&apos;PHP_SELF&apos;] can include arbitrary user input. The documentation should be updated to reflect this.

The request &quot;http://example.com/info.php/attack%20here&quot; will run /info.php, but in Apache $_SERVER[&apos;PHP_SELF&apos;] will equal &quot;/info.php/attack here&quot;. This is a feature, but it means that PHP_SELF must be treated as user input.

The attack string could contain urlencoded HTML and JavaScript (cross-site scripting) or it could contain urlencoded linebreaks (HTTP response-splitting).

The use of $_SERVER[&apos;SCRIPT_NAME&apos;] is recommended instead.

  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.php)

**[To root](/README.md)**