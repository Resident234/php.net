# mt_srand



Looks like mt_rand() gives same result for different seeds when the lowest bits are different only. Try this:<br><br>#!/usr/bin/php -q<br>

```
<?php

$min = -17;
$max = $min + 48; // 48 is to fit the results in my console

for ($testseed=$min; $testseed<$max; $testseed++)
{
    mt_srand( $testseed );
    $r = mt_rand();
    printf("mt_srand( 0x%08x ): mt_rand() == 0x%08x == %d\n", $testseed, $r, $r);
}

?>
```
<br><br>This is a snapshop of the results:<br>...<br>mt_srand( 0xfffffffc ): mt_rand() == 0x0a223d97 == 170016151<br>mt_srand( 0xfffffffd ): mt_rand() == 0x0a223d97 == 170016151<br>mt_srand( 0xfffffffe ): mt_rand() == 0x350a9509 == 889885961<br>mt_srand( 0xffffffff ): mt_rand() == 0x350a9509 == 889885961<br>mt_srand( 0x00000000 ): mt_rand() == 0x71228443 == 1898087491<br>mt_srand( 0x00000001 ): mt_rand() == 0x71228443 == 1898087491<br>mt_srand( 0x00000002 ): mt_rand() == 0x4e0a2cdd == 1309289693<br>mt_srand( 0x00000003 ): mt_rand() == 0x4e0a2cdd == 1309289693<br>...<br><br>I found this occationally. I have no idea if it is a bug or not. In my real life I do not intend to use sequentional seeds. However, probably this may be important for somebody.  

---

[Official documentation page](https://www.php.net/manual/en/function.mt-srand.php)

**[To root](/README.md)**