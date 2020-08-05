# unpack



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
  

---

I had a situation where I had to unpack a file filled with little-endian order double-floats in a way that would work on either little-endian or big-endian machines.  PHP doesn&apos;t have a formatting code that will change the byte order of doubles, so I wrote this workaround.<br><br>

```
<?php
/*The following code is a workaround for php's unpack function
which does not have the capability of unpacking double precision
floats that were packed in the opposite byte order of the current
machine.
*/
function big_endian_unpack ($format, $data) {
    $ar = unpack ($format, $data);
    $vals = array_values ($ar);
    $f = explode ('/', $format);
    $i = 0;
    foreach ($f as $f_k => $f_v) {
    $repeater = intval (substr ($f_v, 1));
    if ($repeater == 0) $repeater = 1;
    if ($f_v{1} == '*')
    {
        $repeater = count ($ar) - $i;
    }
    if ($f_v{0} != 'd') { $i += $repeater; continue; }
    $j = $i + $repeater;
    for ($a = $i; $a < $j; ++$a)
    {
        $p = pack ('d',$vals[$i]);
        $p = strrev ($p);
        list ($vals[$i]) = array_values (unpack ('d1d', $p));
        ++$i;
    }
    }
    $a = 0;
    foreach ($ar as $ar_k => $ar_v) {
    $ar[$ar_k] = $vals[$a];
    ++$a;
    }
    return $ar;
}

list ($endiantest) = array_values (unpack ('L1L', pack ('V',1)));
if ($endiantest != 1) define ('BIG_ENDIAN_MACHINE',1);
if (defined ('BIG_ENDIAN_MACHINE')) $unpack_workaround = 'big_endian_unpack';
else $unpack_workaround = 'unpack';
?>
```


This workaround is used like this:



```
<?php

function foo() {
        global $unpack_workaround;
    $bar = $unpack_workaround('N7N/V2V/d8d',$my_data);
//...
}

?>
```
<br><br>On a little endian machine, $unpack_workaround will simply point to the function unpack.  On a big endian machine, it will call the workaround function.<br><br>Note, this solution only works for doubles.  In my project I had no need to check for single precision floats.  

---

This is about the last example of my previous post. For the sake of clarity, I&apos;m including again here the example, which expands the one given in the formal documentation:<br><br>

```
<?php
  $binarydata = "AA\0A";
  $array = unpack("c2chars/nint", $binarydata);
  foreach ($array as $key => $value)
     echo "\$array[$key] = $value <br>\n";
?>
```
<br><br>This outputs:<br><br>$array[chars1] = 65 <br>$array[chars2] = 65 <br>$array[int] = 65 <br><br>Here, we assume that the ascii code for character &apos;A&apos; is decimal 65.<br><br>Remebering that the format string structure is:<br>&lt;format-code&gt; [&lt;count&gt;] [&lt;array-key&gt;] [/ ...],<br>in this example, the format string instructs the function to<br>  1. ("c2...") Read two chars from the second argument ("AA ...), <br>  2. (...chars...) Use the array-keys "chars1", and "chars2" for <br>      these two chars read,<br>  3. (.../n...) Read a short int from the second argument (...\0A"),<br>  4. (...int") Use the word "int" as the array key for the just read<br>      short.<br><br>I hope this is clearer now,<br><br>Sergio.  

---

[Official documentation page](https://www.php.net/manual/en/function.unpack.php)

**[To root](/README.md)**