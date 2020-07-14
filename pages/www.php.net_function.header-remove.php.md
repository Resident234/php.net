# header_remove



if you want to remove header information about php version (x-powered-by), you can use:<br><br>header_remove(&apos;x-powered-by&apos;);<br><br>alternatively, if you don&apos;t have php 5.3 installed, you can do the same thing using "header" command:<br><br>header(&apos;x-powered-by:&apos;);<br><br>don&apos;t forget the &apos;:&apos; character at the end of the string!  

#

[Official documentation page](https://www.php.net/manual/en/function.header-remove.php)

**[To root](/README.md)**