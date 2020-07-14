# BC Math Functions



Please be aware not to use/have spaces in your strings. It took me a while to find the error in some advanced calculations!<br><br>

```
<?php
echo bcadd("1", "2"); // 3
echo bcadd("1", "2 "); // 1
echo bcadd("1", " 2"); // 1
?>
```
  

#

Here are some useful functions to convert large hex numbers from and to large decimal ones :<br><br>

```
<?php<br>    public static function bchexdec($hex) {<br>        if(strlen($hex) == 1) {<br>            return hexdec($hex);<br>        } else {<br>            $remain = substr($hex, 0, -1);<br>            $last = substr($hex, -1);<br>            return bcadd(bcmul(16, bchexdec($remain)), hexdec($last));<br>        }<br>    }<br><br>    public static function bcdechex($dec) {<br>        $last = bcmod($dec, 16);<br>        $remain = bcdiv(bcsub($dec, $last), 16);<br><br>        if($remain == 0) {<br>            return dechex($last);<br>        } else {<br>            return bcdechex($remain).dechex($last);<br>        }<br>    }  

#

[Official documentation page](https://www.php.net/manual/en/ref.bc.php)

**[To root](/README.md)**