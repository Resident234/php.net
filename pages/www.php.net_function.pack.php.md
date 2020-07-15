# pack



If you&apos;d like to understand pack/unpack. There is a tutorial here in perl, that works equally well in understanding it for php:<br><br>http://perldoc.perl.org/perlpacktut.html  

#

A helper class to convert integer to binary strings and vice versa. Useful for writing and reading integers to / from files or sockets.<br><br>

```
<?php

    class int_helper
    {
        public static function int8($i) {
            return is_int($i) ? pack("c", $i) : unpack("c", $i)[1];
        }

        public static function uInt8($i) {
            return is_int($i) ? pack("C", $i) : unpack("C", $i)[1];
        }

        public static function int16($i) {
            return is_int($i) ? pack("s", $i) : unpack("s", $i)[1];
        }

        public static function uInt16($i, $endianness=false) {
            $f = is_int($i) ? "pack" : "unpack";

            if ($endianness === true) {  // big-endian
                $i = $f("n", $i);
            }
            else if ($endianness === false) {  // little-endian
                $i = $f("v", $i);
            }
            else if ($endianness === null) {  // machine byte order
                $i = $f("S", $i);
            }

            return is_array($i) ? $i[1] : $i;
        }

        public static function int32($i) {
            return is_int($i) ? pack("l", $i) : unpack("l", $i)[1];
        }

        public static function uInt32($i, $endianness=false) {
            $f = is_int($i) ? "pack" : "unpack";

            if ($endianness === true) {  // big-endian
                $i = $f("N", $i);
            }
            else if ($endianness === false) {  // little-endian
                $i = $f("V", $i);
            }
            else if ($endianness === null) {  // machine byte order
                $i = $f("L", $i);
            }

            return is_array($i) ? $i[1] : $i;
        }

        public static function int64($i) {
            return is_int($i) ? pack("q", $i) : unpack("q", $i)[1];
        }

        public static function uInt64($i, $endianness=false) {
            $f = is_int($i) ? "pack" : "unpack";

            if ($endianness === true) {  // big-endian
                $i = $f("J", $i);
            }
            else if ($endianness === false) {  // little-endian
                $i = $f("P", $i);
            }
            else if ($endianness === null) {  // machine byte order
                $i = $f("Q", $i);
            }

            return is_array($i) ? $i[1] : $i;
        }
    }
?>
```


Usage example:


```
<?php
    Header("Content-Type: text/plain");
    include("int_helper.php");

    echo int_helper::uInt8(0x6b) . PHP_EOL;  // k
    echo int_helper::uInt8(107) . PHP_EOL;  // k
    echo int_helper::uInt8("\x6b") . PHP_EOL . PHP_EOL;  // 107

    echo int_helper::uInt16(4101) . PHP_EOL;  // \x05\x10
    echo int_helper::uInt16("\x05\x10") . PHP_EOL;  // 4101
    echo int_helper::uInt16("\x05\x10", true) . PHP_EOL . PHP_EOL;  // 1296

    echo int_helper::uInt32(2147483647) . PHP_EOL;  // \xff\xff\xff\x7f
    echo int_helper::uInt32("\xff\xff\xff\x7f") . PHP_EOL . PHP_EOL;  // 2147483647

    // Note: Test this with 64-bit build of PHP
    echo int_helper::uInt64(9223372036854775807) . PHP_EOL;  // \xff\xff\xff\xff\xff\xff\xff\x7f
    echo int_helper::uInt64("\xff\xff\xff\xff\xff\xff\xff\x7f") . PHP_EOL . PHP_EOL;  // 9223372036854775807

?>
```
  

#

Note that the the upper command in perl looks like this:<br><br>$binarydata = pack ("n v c*", 0x1234, 0x5678, 65, 66);<br>In PHP it seems that no whitespaces are allowed in the first parameter. So if you want to convert your pack command from perl -&gt; PHP, don&apos;t forget to remove the whitespaces!  

#

If you need to unpack a signed short from big-endian or little-endian specifically, instead of machine-byte-order, you need only unpack it as the unsigned form, and then if the result is &gt;= 2^15, subtract 2^16 from it.<br><br>And example would be:<br><br>

```
<?php
$foo = unpack("n", $signedbigendianshort);
$foo = $foo[1];
if($foo >= pow(2, 15)) $foo -= pow(2, 16);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.pack.php)

**[To root](/README.md)**