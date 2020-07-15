# hash



I was interested how "long" each hash is, so I did:<br><br>

```
<?php
$data = "hello";

foreach (hash_algos() as $v) {
        $r = hash($v, $data, false);
        printf("%-12s %3d %s\n", $v, strlen($r), $r);
}
?>
```
<br><br>which produce (long hashes are cropped)<br><br>md2           32 a9046c73e00331af68917d3804f70655                   <br>md4           32 866437cb7a794bce2b727acc0362ee27<br>md5           32 5d41402abc4b2a76b9719d911017c592<br>sha1          40 aaf4c61ddcc5e8a2dabede0f3b482cd9aea9434d<br>sha256        64 2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e730<br>sha384        96 59e1748777448c69de6b800d7a33bbfb9ff1b463e44354c3553<br>sha512       128 9b71d224bd62f3785d96d46ad3ea3d73319bfbc2890caadae2d<br>ripemd128     32 789d569f08ed7055e94b4289a4195012<br>ripemd160     40 108f07b8382412612c048d07d13f814118445acd<br>ripemd256     64 cc1d2594aece0a064b7aed75a57283d9490fd5705ed3d66bf9a<br>ripemd320     80 eb0cf45114c56a8421fbcb33430fa22e0cd607560a88bbe14ce<br>whirlpool    128 0a25f55d7308eca6b9567a7ed3bd1b46327f0f1ffdc804dd8bb<br>tiger128,3    32 a78862336f7ffd2c8a3874f89b1b74f2<br>tiger160,3    40 a78862336f7ffd2c8a3874f89b1b74f2f27bdbca<br>tiger192,3    48 a78862336f7ffd2c8a3874f89b1b74f2f27bdbca39660254<br>tiger128,4    32 1c2a939f230ee5e828f5d0eae5947135<br>tiger160,4    40 1c2a939f230ee5e828f5d0eae5947135741cd0ae<br>tiger192,4    48 1c2a939f230ee5e828f5d0eae5947135741cd0aefeeb2adc<br>snefru        64 7c5f22b1a92d9470efea37ec6ed00b2357a4ce3c41aa6e28e3b<br>gost          64 a7eb5d08ddf2363f1ea0317a803fcef81d33863c8b2f9f6d7d1<br>adler32        8 062c0215<br>crc32          8 3d653119<br>crc32b         8 3610a686<br>haval128,3    32 85c3e4fac0ba4d85519978fdc3d1d9be<br>haval160,3    40 0e53b29ad41cea507a343cdd8b62106864f6b3fe<br>haval192,3    48 bfaf81218bbb8ee51b600f5088c4b8601558ff56e2de1c4f<br>haval224,3    56 92d0e3354be5d525616f217660e0f860b5d472a9cb99d6766be<br>haval256,3    64 26718e4fb05595cb8703a672a8ae91eea071cac5e7426173d4c<br>haval128,4    32 fe10754e0b31d69d4ece9c7a46e044e5<br>haval160,4    40 b9afd44b015f8afce44e4e02d8b908ed857afbd1<br>haval192,4    48 ae73833a09e84691d0214f360ee5027396f12599e3618118<br>haval224,4    56 e1ad67dc7a5901496b15dab92c2715de4b120af2baf661ecd92<br>haval256,4    64 2d39577df3a6a63168826b2a10f07a65a676f5776a0772e0a87<br>haval128,5    32 d20e920d5be9d9d34855accb501d1987<br>haval160,5    40 dac5e2024bfea142e53d1422b90c9ee2c8187cc6<br>haval192,5    48 bbb99b1e989ec3174019b20792fd92dd67175c2ff6ce5965<br>haval224,5    56 aa6551d75e33a9c5cd4141e9a068b1fc7b6d847f85c3ab16295<br>haval256,5    64 348298791817d5088a6de6c1b6364756d404a50bd64e645035f  

#

I was curious about hashes behavior (time and length), so i ran this code (yeah, I&apos;m cheating because i took two examples from this page)<br>

```
<?php
  echo "Building data...";
  $data = "";
  for($i = 0; $i < 1500; $i++)
    $data .= sha1("H:k - $i - k:H");

  echo "OK! (".strlen($data)." bytes)".PHP_EOL;

  $res = [];

  echo "Testing hashes.....".PHP_EOL;

  foreach (hash_algos() as $algo) {
    $time = microtime(1);
    $hash = hash($algo, $data);
    $time = (microtime(1) - $time) * 1000;
    $length = strlen($hash);

    $res["$time"][] = [
      "algo"   => "HEX-$algo",
      "length" => "$length",
      "time"   => sprintf("%.8f", $time)
    ];

    $time = microtime(1);
    hash($algo, $data, 1);
    $time = (microtime(1) - $time) * 1000;

    $res["$time"][] = [
      "algo"   => "RAW-$algo",
      "length" => "$length",
      "time"   => sprintf("%.8f", $time)
    ];
  }

  ksort($res);
  $i = 0;

  echo "Results:".PHP_EOL;
  echo "Posit.      Time in ms   Type-Hash algo        Hash length".PHP_EOL;

  foreach($res as $sres){
    foreach($sres as $result){
      echo sprintf("%5d. %12s ms    %-20s %-2d bytes", $i++, $result['time'], $result['algo'], $result['length']).PHP_EOL;
    }
  }
?>
```
<br><br>I found interesting that today (may 31, 2018), I found 103 algos, 65 more than 9 years ago (based on this comment: https://secure.php.net/manual/en/function.hash.php#89574 )<br><br>Also, here is my results: https://ideone.com/embed/0iwuGn<br><br>I&apos;m not posting here due to message length limitations.  

#

The well known hash functions MD5 and SHA1 should be avoided in new applications. Collission attacks against MD5 are well documented in the cryptographics literature and have already been demonstrated in practice. Therefore, MD5 is no longer secure for certain applications.<br><br>Collission attacks against SHA1 have also been published, though they still require computing power, which is somewhat out of scope. As computing power increases with time and the attacks are likely to get better, too, attacks against systems relying on SHA1 for security are likely to become feasible within the next few years.<br><br>There is no lack of potential alternative hash algorithms, as the many choices for the "algo" argument of PHPs hash() function already suggests. Unfortunately, there is lack of analysis, as to how secure these alternative algorithms are. It is rather safe to assume, though, that the SHA2 family with its most prominent members SHA-256 und SHA-512, is better than SHA1.<br><br>When storing password hashes, it is a good idea to prefix a salt to the password before hashing, to avoid the same passwords to hash to the same values and to avoid the use of rainbow tables for password recovery. Unlike suggested in other articles, there is no security advantage in putting the salt in the middle, or even at both the beginning and the end, of the combined salt-password-string.<br><br>Rather, there are two other factors, that determine the strength of the salt: Its length and its variability. For example, using the same salt for all passwords is easy to implement, but gives only very little additional security. In particular, if users type the same passwords, they will still hash to the same value!<br><br>Therefore, the salt should be random string with at least as many variable bits, as there are bits in the hash result. In the user database, store username, the randomly generated salt for that user, and the result of hashing the salt-password-string. Access authentication is then done by looking up the entry for the user, calculating the hash of the salt found in the database and the password provided by the user, and comparing the result with the one stored in the database.  

#

Performance test results on my laptop:<br>Results are here shorten to fit php web notes ...<br>This was tested with 1024000 bytes (1000 KB) of random data, md4 always gets the first place, and md2 always get the last place :)<br><br>Results: (in microseconds)<br>   1.  md4                           5307.912<br>   2.  md5                           6890.058<br>   3.  crc32b                        7298.946<br>   4.  crc32                         7561.922<br>   5.  sha1                          8886.098<br>   6.  tiger128,3                    11054.992<br>   7.  haval192,3                    11132.955<br>   8.  haval224,3                    11160.135<br>   9.  tiger160,3                    11162.996<br>  10.  haval160,3                    11242.151<br>  11.  haval256,3                    11327.981<br>  12.  tiger192,3                    11630.058<br>  13.  haval128,3                    11880.874<br>  14.  tiger192,4                    14776.945<br>  15.  tiger128,4                    14871.12<br>  16.  tiger160,4                    14946.937<br>  17.  haval160,4                    15661.954<br>  18.  haval192,4                    15717.029<br>  19.  haval256,4                    15759.944<br>  20.  adler32                       15796.184<br>  21.  haval128,4                    15887.022<br>  22.  haval224,4                    16047.954<br>  23.  ripemd256                     16245.126<br>  24.  haval160,5                    17818.927<br>  25.  haval128,5                    17887.115<br>  26.  haval224,5                    18085.002<br>  27.  haval192,5                    18135.07<br>  28.  haval256,5                    18678.903<br>  29.  sha256                        19020.08<br>  30.  ripemd128                     20671.844<br>  31.  ripemd160                     21853.923<br>  32.  ripemd320                     22425.889<br>  33.  sha384                        45102.119<br>  34.  sha512                        45655.965<br>  35.  gost                          57237.148<br>  36.  whirlpool                     64682.96<br>  37.  snefru                        80352.783<br>  38.  md2                           705397.844<br><br>Code for generating this:<br>(compatible both with browser and cli mode)<br><br>&lt;pre&gt;<br><br>

```
<?php

echo 'Building random data ...' . PHP_EOL; 
@ob_flush();flush();

$data = '';
for ($i = 0; $i < 64000; $i++)
    $data .= hash('md5', rand(), true);

echo strlen($data) . ' bytes of random data built !' . PHP_EOL . PHP_EOL . 'Testing hash algorithms ...' . PHP_EOL; 
@ob_flush();flush();

$results = array();
foreach (hash_algos() as $v) {
    echo $v . PHP_EOL; 
    @ob_flush();flush();
    $time = microtime(true);
    hash($v, $data, false);
    $time = microtime(true) - $time;
    $results[$time * 1000000000][] = "$v (hex)";
    $time = microtime(true);
    hash($v, $data, true);
    $time = microtime(true) - $time;
    $results[$time * 1000000000][] = "$v (raw)";
}

ksort($results);

echo PHP_EOL . PHP_EOL . 'Results: ' . PHP_EOL; 

$i = 1;
foreach ($results as $k => $v)
    foreach ($v as $k1 => $v1)
        echo ' ' . str_pad($i++ . '.', 4, ' ', STR_PAD_LEFT) . '  ' . str_pad($v1, 30, ' ') . ($k / 1000) . ' microseconds' . PHP_EOL;

?>
```
<br><br>&lt;/pre&gt;  

#

Just a quick note about these benchmarks and how you should apply them.<br><br>If you are hashing passwords etc for security, speed is not your friend. You should use the slowest method.<br><br>Slow to hash means slow to crack and will hopefully make generating things like rainbow tables more trouble than it&apos;s worth.  

#

[Official documentation page](https://www.php.net/manual/en/function.hash.php)

**[To root](/README.md)**