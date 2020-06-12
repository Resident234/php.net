# Escape sequences




<div class="phpcode"><span class="html">
&quot;line break&quot; is ill-defined:<br><br> -- Windows uses CR+LF (\r\n)<br> -- Linux LF (\n)<br> -- OSX CR (\r)<br><br>Little-known special character:<br>\R in preg_* matches all three.<br><br>preg_match( &apos;/^\R$/&apos;, &quot;match\nany\\n\rline\r\nending\r&quot; ); // match any line endings</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/regexp.reference.escape.php)

**[⬆ to root](/)**