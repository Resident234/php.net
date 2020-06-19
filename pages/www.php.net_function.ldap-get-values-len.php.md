# ldap_get_values_len




<div class="phpcode"><span class="html">
Revisiting the question of converting a binary objectGUID to a string, there&apos;s a simpler method that doesn&apos;t involve a lot of string manipulation:<br><br>function GUIDtoStr($binary_guid) {<br>&#xA0; $unpacked = unpack(&apos;Va/v2b/n2c/Nd&apos;, $binary_guid);<br>&#xA0; return sprintf(&apos;%08X-%04X-%04X-%04X-%04X%08X&apos;, $unpacked[&apos;a&apos;], $unpacked[&apos;b1&apos;], $unpacked[&apos;b2&apos;], $unpacked[&apos;c1&apos;], $unpacked[&apos;c2&apos;], $unpacked[&apos;d&apos;]);<br>}<br><br>You can of course replace &quot;X&quot; with &quot;x&quot; in the sprintf format string if you prefer lower-case hex digits.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ldap-get-values-len.php)

**[To root](/README.md)**