# mssql_guid_string



Since this was removed in PHP7. You can achieve the same output with the following code. <br><br>function convertBinToMSSQLGuid($binguid)<br>{<br>        $unpacked = unpack(&apos;Va/v2b/n2c/Nd&apos;, $binguid);<br>        return sprintf(&apos;%08X-%04X-%04X-%04X-%04X%08X&apos;, $unpacked[&apos;a&apos;], $unpacked[&apos;b1&apos;], $unpacked[&apos;b2&apos;], $unpacked[&apos;c1&apos;], $unpacked[&apos;c2&apos;], $unpacked[&apos;d&apos;]);<br>}<br>Enjoy.  

#

[Official documentation page](https://www.php.net/manual/en/function.mssql-guid-string.php)

**[To root](/README.md)**