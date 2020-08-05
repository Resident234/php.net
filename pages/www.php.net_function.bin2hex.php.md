# bin2hex



This function is for converting binary data into a hexadecimal string representation.  This function is not for converting strings representing binary digits into hexadecimal.  If you want that functionality, you can simply do this:<br><br>

```
<?php
$binary = "11111001";
$hex = dechex(bindec($binary));
echo $hex;
?>
```
<br><br>This would output "f9".  Just remember that there is a very big difference between binary data and a string representation of binary.  

---

[Official documentation page](https://www.php.net/manual/en/function.bin2hex.php)

**[To root](/README.md)**