# Dealing with XML errors



Note that "if (! $sxe) {" may give you a false-negative if the XML document was empty (e.g. "&lt;root /&gt;").  In that case, $sxe will be:<br><br>object(SimpleXMLElement)#1 (0) {<br>}<br><br>which will evaluate to false, even though nothing technically went wrong.<br><br>Consider instead: "if ($sxe === false) {"  

#

[Official documentation page](https://www.php.net/manual/en/simplexml.examples-errors.php)

**[To root](/README.md)**