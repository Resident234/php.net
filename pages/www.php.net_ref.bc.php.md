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
<?php
    public static function bchexdec($hex) {
        if(strlen($hex) == 1) {
            return hexdec($hex);
        } else {
            $remain = substr($hex, 0, -1);
            $last = substr($hex, -1);
            return bcadd(bcmul(16, bchexdec($remain)), hexdec($last));
        }
    }

    public static function bcdechex($dec) {
        $last = bcmod($dec, 16);
        $remain = bcdiv(bcsub($dec, $last), 16);

        if($remain == 0) {
            return dechex($last);
        } else {
            return bcdechex($remain).dechex($last);
        }
    }?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/ref.bc.php)

**[To root](/README.md)**