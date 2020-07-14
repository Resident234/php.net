# ldap_get_values_len



Revisiting the question of converting a binary objectGUID to a string, there&apos;s a simpler method that doesn&apos;t involve a lot of string manipulation:<br><br>function GUIDtoStr($binary_guid) {<br>  $unpacked = unpack(&apos;Va/v2b/n2c/Nd&apos;, $binary_guid);<br>  return sprintf(&apos;%08X-%04X-%04X-%04X-%04X%08X&apos;, $unpacked[&apos;a&apos;], $unpacked[&apos;b1&apos;], $unpacked[&apos;b2&apos;], $unpacked[&apos;c1&apos;], $unpacked[&apos;c2&apos;], $unpacked[&apos;d&apos;]);<br>}<br><br>You can of course replace "X" with "x" in the sprintf format string if you prefer lower-case hex digits.  

#

[Official documentation page](https://www.php.net/manual/en/function.ldap-get-values-len.php)

**[To root](/README.md)**