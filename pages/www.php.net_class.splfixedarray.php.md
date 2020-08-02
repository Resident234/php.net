# The SplFixedArray class



As the documentation says, SplFixedArray is meant to be *faster* than array. Do not blindly believe other people&apos;s benchmarks, and beextra careful with the user comments on php.net. For instance, nairbv&apos;s benchmark code is completely wrong. Among other errors, it intends to increase the size of the arrays, but always initialize a 20 elements SplFixedArray.<br><br>On a PHP 5.4 64 bits linux server, I found SplFixedArray to be always faster than array().<br>* small data (1,000):<br>    * write: SplFixedArray is 15 % faster<br>    * read:  SplFixedArray is  5 % faster<br>* larger data (512,000):<br>    * write: SplFixedArray is 33 % faster<br>    * read:  SplFixedArray is 10 % faster  

---

Note, that this is considerably faster and should be used when the size of the array is known. Here are some very basic bench marks:<br><br>

```
<?php
for($size = 1000; $size < 50000000; $size *= 2) {
    echo PHP_EOL . "Testing size: $size" . PHP_EOL;
    for($s = microtime(true), $container = Array(), $i = 0; $i < $size; $i++) $container[$i] = NULL;
    echo "Array(): " . (microtime(true) - $s) . PHP_EOL;

    for($s = microtime(true), $container = new SplFixedArray($size), $i = 0; $i < $size; $i++) $container[$i] = NULL;
    echo "SplArray(): " . (microtime(true) - $s) . PHP_EOL;
}
?>
```
<br><br>OUTPUT<br>Testing size: 1000<br>Array(): 0.00046396255493164<br>SplArray(): 0.00023293495178223<br><br>Testing size: 2000<br>Array(): 0.00057101249694824<br>SplArray(): 0.0003058910369873<br><br>Testing size: 4000<br>Array(): 0.0015869140625<br>SplArray(): 0.00086307525634766<br><br>Testing size: 8000<br>Array(): 0.0024251937866211<br>SplArray(): 0.00211501121521<br><br>Testing size: 16000<br>Array(): 0.0057680606842041<br>SplArray(): 0.0041120052337646<br><br>Testing size: 32000<br>Array(): 0.011334896087646<br>SplArray(): 0.007631778717041<br><br>Testing size: 64000<br>Array(): 0.021990060806274<br>SplArray(): 0.013560056686401<br><br>Testing size: 128000<br>Array(): 0.053267002105713<br>SplArray(): 0.030976057052612<br><br>Testing size: 256000<br>Array(): 0.10280108451843<br>SplArray(): 0.056283950805664<br><br>Testing size: 512000<br>Array(): 0.20657992362976<br>SplArray(): 0.11510300636292<br><br>Testing size: 1024000<br>Array(): 0.4138810634613<br>SplArray(): 0.21826505661011<br><br>Testing size: 2048000<br>Array(): 0.85640096664429<br>SplArray(): 0.46247816085815<br><br>Testing size: 4096000<br>Array(): 1.7242450714111<br>SplArray(): 0.95304894447327<br><br>Testing size: 8192000<br>Array(): 3.448086977005<br>SplArray(): 1.96746301651  

---

Memory footprint of splFixedArray is about 37% of a regular "array" of the same size. <br>I was hoping for more, but that&apos;s also significant, and that&apos;s where you should expect to see difference, not in "performance".  

---

[Official documentation page](https://www.php.net/manual/en/class.splfixedarray.php)

**[To root](/README.md)**