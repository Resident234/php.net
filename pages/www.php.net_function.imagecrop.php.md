# imagecrop





It appears that imagecrop() will output a black line along the bottom the resulting image until version 5.6.12. Your only choices are to upgrade PHP or use imagecopyresampled().

http://php.net/ChangeLog-5.php#5.6.12 (bug #67447)

  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecrop.php)

**[To root](/README.md)**