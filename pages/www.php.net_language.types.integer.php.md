# Integers



A leading zero in a numeric literal means "this is octal". But don&apos;t be confused: a leading zero in a string does not. Thus:<br>$x = 0123;          // 83<br>$y = "0123" + 0     // 123  

---

Here are some tricks to convert from a "dotted" IP address to a LONG int, and backwards. This is very useful because accessing an IP addy in a database table is very much faster if it&apos;s stored as a BIGINT rather than in characters.<br><br>IP to BIGINT:<br>

```
<?php
  $ipArr    = explode('.',$_SERVER['REMOTE_ADDR']);
  $ip       = $ipArr[0] * 0x1000000
            + $ipArr[1] * 0x10000
            + $ipArr[2] * 0x100
            + $ipArr[3]
            ;
?>
```


IP as BIGINT read from db back to dotted form:

Keep in mind, PHP integer operators are INTEGER -- not long. Also, since there is no integer divide in PHP, we save a couple of S-L-O-W floor (<division>)'s by doing bitshifts. We must use floor(/) for $ipArr[0] because though $ipVal is stored as a long value, $ipVal >> 24 will operate on a truncated, integer value of $ipVal! $ipVint is, however, a nice integer, so 
we can enjoy the bitshifts.



```
<?php
        $ipVal = $row['client_IP'];
        $ipArr = array(0 =>
                    floor(  $ipVal               / 0x1000000) );
        $ipVint   = $ipVal-($ipArr[0]*0x1000000); // for clarity
        $ipArr[1] = ($ipVint &amp; 0xFF0000)  >> 16;
        $ipArr[2] = ($ipVint &amp; 0xFF00  )  >> 8;
        $ipArr[3] =  $ipVint &amp; 0xFF;
        $ipDotted = implode('.', $ipArr);
?>
```
  

---

-------------------------------------------------------------------------<br>Question : <br>var_dump((int) 010);  //Output 8<br><br>var_dump((int) "010"); //output 10<br><br>First one is octal notation so the output is correct. But what about the when converting "010" to integer. it should be also output 8 ?<br>--------------------------------------------------------------------------<br>Answer :<br><br>Casting to an integer using (int) will always cast to the default base, which is 10.<br><br>Casting a string to a number this way does not take into account the many ways of formatting an integer value in PHP (leading zero for base 8, leading "0x" for base 16, leading "0b" for base 2). It will simply look at the first characters in a string and convert them to a base 10 integer. Leading zeroes will be stripped off because they have no meaning in numerical values, so you will end up with the decimal value 10 for (int)"010".<br><br>Converting an integer value between bases using (int)010 will take into account the various ways of formatting an integer. A leading zero like in 010 means the number is in octal notation, using (int)010 will convert it to the decimal value 8 in base 10.<br><br>This is similar to how you use 0x10 to write in hexadecimal (base 16) notation. Using (int)0x10 will convert that to the base 10 decimal value 16, whereas using (int)"0x10" will end up with the decimal value 0: since the "x" is not a numerical value, anything after that will be ignored.<br><br>If you want to interpret the string "010" as an octal value, you need to instruct PHP to do so. intval("010", 8) will interpret the number in base 8 instead of the default base 10, and you will end up with the decimal value 8. You could also use octdec("010") to convert the octal string to the decimal value 8. Another option is to use base_convert("010", 8, 10) to explicitly convert the number "010" from base 8 to base 10, however this function will return the string "8" instead of the integer 8.<br><br>Casting a string to an integer follows the same the logic used by the intval function:<br><br>Returns the integer value of var, using the specified base for the conversion (the default is base 10).<br>intval allows specifying a different base as the second argument, whereas a straight cast operation does not, so using (int) will always treat a string as being in base 10.<br><br>php &gt; var_export((int) "010");<br>10<br>php &gt; var_export(intval("010"));<br>10<br>php &gt; var_export(intval("010", 8));<br>8  

---

"There is no integer division operator in PHP". But since PHP 7, there is the intdiv function.  

---

Converting to an integer works only if the input begins with a number<br>(int) "5txt" // will output the integer 5<br>(int) "before5txt" // will output the integer 0<br>(int) "53txt" // will output the integer 53<br>(int) "53txt534text" // will output the integer 53  

---

Be careful with using the modulo operation on big numbers, it will cast a float argument to an int and may return wrong results. For example:<br>

```
<?php
    $i = 6887129852;
    echo "i=$i\n";
    echo "i%36=".($i%36)."\n";
    echo "alternative i%36=".($i-floor($i/36)*36)."\n";
?>
```
<br>Will output:<br>i=6.88713E+009<br>i%36=-24<br>alternative i%36=20  

---

[Official documentation page](https://www.php.net/manual/en/language.types.integer.php)

**[To root](/README.md)**