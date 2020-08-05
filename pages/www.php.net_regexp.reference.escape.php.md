# Escape sequences



"line break" is ill-defined:<br><br> -- Windows uses CR+LF (\r\n)<br> -- Linux LF (\n)<br> -- OSX CR (\r)<br><br>Little-known special character:<br>\R in preg_* matches all three.<br><br>preg_match( &apos;/^\R$/&apos;, "match\nany\\n\rline\r\nending\r" ); // match any line endings  

---

[Official documentation page](https://www.php.net/manual/en/regexp.reference.escape.php)

**[To root](/README.md)**