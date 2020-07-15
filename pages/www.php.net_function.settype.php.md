# settype



Note that you can&apos;t use this to convert a string &apos;true&apos; or &apos;false&apos; to a boolean variable true or false as a string &apos;false&apos; is a boolean true. The empty string would be false instead...<br><br>

```
<?php
$var = "true";
settype($var, 'bool');
var_dump($var); // true

$var = "false";
settype($var, 'bool');
var_dump($var); // true as well!

$var = "";
settype($var, 'bool');
var_dump($var); // false
?>
```
  

#

Just a quick note, as this caught me out very briefly:<br><br>settype() returns bool, not the typecasted variable - so:<br><br>$blah = settype($blah, "int"); // is wrong, changes $blah to 0 or 1<br>settype($blah, "int"); // is correct<br><br>Hope this helps someone else who makes a mistake.. ;)  

#

Using settype is not the best way to convert a string into an integer, since it will strip the string wherever the first non-numeric character begins.  The function intval($string) does the same thing.<br><br>If you&apos;re looking for a security check, or to strip non-numeric characters (such as cleaning up phone numbers or ZIP codes),  try this instead:<br><br>&lt;?<br>     $number=ereg_replace("[^0-9]","",$number);<br>?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.settype.php)

**[To root](/README.md)**