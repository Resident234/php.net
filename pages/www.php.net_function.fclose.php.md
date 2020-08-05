# fclose



It is a GOOD_THING to check the return value from fclose(), as some operating systems only flush file output on close, and can, therefore, return an error from fclose(). You can catch severe data-eating errors by doing this. <br><br>I learned this the hard way.  

---

[Official documentation page](https://www.php.net/manual/en/function.fclose.php)

**[To root](/README.md)**