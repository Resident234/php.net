# convert_uuencode



@Craig&apos;s note: base64_encode() is better suited for that. In fact, it produces smaller output and operates slightly faster. I did a little benchmark -- here are my findings:<br><br>File: JPG, 631614 bytes<br><br>== Base64 ==<br>execution time: 0.0039639472961426 secs<br>output length: 842152<br><br>== UUencode ==<br>execution time: 0.004105806350708 secs<br>output length: 870226  

#

[Official documentation page](https://www.php.net/manual/en/function.convert-uuencode.php)

**[To root](/README.md)**