# Predefined Variables




<div class="phpcode"><span class="html">
Warning: $_SERVER[&apos;PHP_SELF&apos;] can include arbitrary user input. The documentation should be updated to reflect this.<br><br>The request &quot;<a href="http://example.com/info.php/attack%20here" rel="nofollow" target="_blank">http://example.com/info.php/attack%20here</a>&quot; will run /info.php, but in Apache $_SERVER[&apos;PHP_SELF&apos;] will equal &quot;/info.php/attack here&quot;. This is a feature, but it means that PHP_SELF must be treated as user input.<br><br>The attack string could contain urlencoded HTML and JavaScript (cross-site scripting) or it could contain urlencoded linebreaks (HTTP response-splitting).<br><br>The use of $_SERVER[&apos;SCRIPT_NAME&apos;] is recommended instead.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.php)

**[⬆ to root](/)**