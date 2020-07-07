# hash





I was interested how &quot;long&quot; each hash is, so I did:





```
<?php

$data = &quot;hello&quot;;



foreach (hash_algos() as $v) {

&#xA0; &#xA0; &#xA0; &#xA0; $r = hash($v, $data, false);

&#xA0; &#xA0; &#xA0; &#xA0; printf(&quot;%-12s %3d %s\n&quot;, $v, strlen($r), $r);

}

?>
```




which produce (long hashes are cropped)



md2&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 32 a9046c73e00331af68917d3804f70655&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 

md4&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 32 866437cb7a794bce2b727acc0362ee27

md5&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 32 5d41402abc4b2a76b9719d911017c592

sha1&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 40 aaf4c61ddcc5e8a2dabede0f3b482cd9aea9434d

sha256&#xA0; &#xA0; &#xA0; &#xA0; 64 2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e730

sha384&#xA0; &#xA0; &#xA0; &#xA0; 96 59e1748777448c69de6b800d7a33bbfb9ff1b463e44354c3553

sha512&#xA0; &#xA0; &#xA0;&#xA0; 128 9b71d224bd62f3785d96d46ad3ea3d73319bfbc2890caadae2d

ripemd128&#xA0; &#xA0;&#xA0; 32 789d569f08ed7055e94b4289a4195012

ripemd160&#xA0; &#xA0;&#xA0; 40 108f07b8382412612c048d07d13f814118445acd

ripemd256&#xA0; &#xA0;&#xA0; 64 cc1d2594aece0a064b7aed75a57283d9490fd5705ed3d66bf9a

ripemd320&#xA0; &#xA0;&#xA0; 80 eb0cf45114c56a8421fbcb33430fa22e0cd607560a88bbe14ce

whirlpool&#xA0; &#xA0; 128 0a25f55d7308eca6b9567a7ed3bd1b46327f0f1ffdc804dd8bb

tiger128,3&#xA0; &#xA0; 32 a78862336f7ffd2c8a3874f89b1b74f2

tiger160,3&#xA0; &#xA0; 40 a78862336f7ffd2c8a3874f89b1b74f2f27bdbca

tiger192,3&#xA0; &#xA0; 48 a78862336f7ffd2c8a3874f89b1b74f2f27bdbca39660254

tiger128,4&#xA0; &#xA0; 32 1c2a939f230ee5e828f5d0eae5947135

tiger160,4&#xA0; &#xA0; 40 1c2a939f230ee5e828f5d0eae5947135741cd0ae

tiger192,4&#xA0; &#xA0; 48 1c2a939f230ee5e828f5d0eae5947135741cd0aefeeb2adc

snefru&#xA0; &#xA0; &#xA0; &#xA0; 64 7c5f22b1a92d9470efea37ec6ed00b2357a4ce3c41aa6e28e3b

gost&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 64 a7eb5d08ddf2363f1ea0317a803fcef81d33863c8b2f9f6d7d1

adler32&#xA0; &#xA0; &#xA0; &#xA0; 8 062c0215

crc32&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 8 3d653119

crc32b&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 8 3610a686

haval128,3&#xA0; &#xA0; 32 85c3e4fac0ba4d85519978fdc3d1d9be

haval160,3&#xA0; &#xA0; 40 0e53b29ad41cea507a343cdd8b62106864f6b3fe

haval192,3&#xA0; &#xA0; 48 bfaf81218bbb8ee51b600f5088c4b8601558ff56e2de1c4f

haval224,3&#xA0; &#xA0; 56 92d0e3354be5d525616f217660e0f860b5d472a9cb99d6766be

haval256,3&#xA0; &#xA0; 64 26718e4fb05595cb8703a672a8ae91eea071cac5e7426173d4c

haval128,4&#xA0; &#xA0; 32 fe10754e0b31d69d4ece9c7a46e044e5

haval160,4&#xA0; &#xA0; 40 b9afd44b015f8afce44e4e02d8b908ed857afbd1

haval192,4&#xA0; &#xA0; 48 ae73833a09e84691d0214f360ee5027396f12599e3618118

haval224,4&#xA0; &#xA0; 56 e1ad67dc7a5901496b15dab92c2715de4b120af2baf661ecd92

haval256,4&#xA0; &#xA0; 64 2d39577df3a6a63168826b2a10f07a65a676f5776a0772e0a87

haval128,5&#xA0; &#xA0; 32 d20e920d5be9d9d34855accb501d1987

haval160,5&#xA0; &#xA0; 40 dac5e2024bfea142e53d1422b90c9ee2c8187cc6

haval192,5&#xA0; &#xA0; 48 bbb99b1e989ec3174019b20792fd92dd67175c2ff6ce5965

haval224,5&#xA0; &#xA0; 56 aa6551d75e33a9c5cd4141e9a068b1fc7b6d847f85c3ab16295

haval256,5&#xA0; &#xA0; 64 348298791817d5088a6de6c1b6364756d404a50bd64e645035f

  

#



I was curious about hashes behavior (time and length), so i ran this code (yeah, I&apos;m cheating because i took two examples from this page)


```
<?php
&#xA0; echo &quot;Building data...&quot;;
&#xA0; $data = &quot;&quot;;
&#xA0; for($i = 0; $i &lt; 1500; $i++)
&#xA0; &#xA0; $data .= sha1(&quot;H:k - $i - k:H&quot;);

&#xA0; echo &quot;OK! (&quot;.strlen($data).&quot; bytes)&quot;.PHP_EOL;

&#xA0; $res = [];

&#xA0; echo &quot;Testing hashes.....&quot;.PHP_EOL;

&#xA0; foreach (hash_algos() as $algo) {
&#xA0; &#xA0; $time = microtime(1);
&#xA0; &#xA0; $hash = hash($algo, $data);
&#xA0; &#xA0; $time = (microtime(1) - $time) * 1000;
&#xA0; &#xA0; $length = strlen($hash);

&#xA0; &#xA0; $res[&quot;$time&quot;][] = [
&#xA0; &#xA0; &#xA0; &quot;algo&quot;&#xA0;&#xA0; =&gt; &quot;HEX-$algo&quot;,
&#xA0; &#xA0; &#xA0; &quot;length&quot; =&gt; &quot;$length&quot;,
&#xA0; &#xA0; &#xA0; &quot;time&quot;&#xA0;&#xA0; =&gt; sprintf(&quot;%.8f&quot;, $time)
&#xA0; &#xA0; ];

&#xA0; &#xA0; $time = microtime(1);
&#xA0; &#xA0; hash($algo, $data, 1);
&#xA0; &#xA0; $time = (microtime(1) - $time) * 1000;

&#xA0; &#xA0; $res[&quot;$time&quot;][] = [
&#xA0; &#xA0; &#xA0; &quot;algo&quot;&#xA0;&#xA0; =&gt; &quot;RAW-$algo&quot;,
&#xA0; &#xA0; &#xA0; &quot;length&quot; =&gt; &quot;$length&quot;,
&#xA0; &#xA0; &#xA0; &quot;time&quot;&#xA0;&#xA0; =&gt; sprintf(&quot;%.8f&quot;, $time)
&#xA0; &#xA0; ];
&#xA0; }

&#xA0; ksort($res);
&#xA0; $i = 0;

&#xA0; echo &quot;Results:&quot;.PHP_EOL;
&#xA0; echo &quot;Posit.&#xA0; &#xA0; &#xA0; Time in ms&#xA0;&#xA0; Type-Hash algo&#xA0; &#xA0; &#xA0; &#xA0; Hash length&quot;.PHP_EOL;

&#xA0; foreach($res as $sres){
&#xA0; &#xA0; foreach($sres as $result){
&#xA0; &#xA0; &#xA0; echo sprintf(&quot;%5d. %12s ms&#xA0; &#xA0; %-20s %-2d bytes&quot;, $i++, $result[&apos;time&apos;], $result[&apos;algo&apos;], $result[&apos;length&apos;]).PHP_EOL;
&#xA0; &#xA0; }
&#xA0; }
?>
```


I found interesting that today (may 31, 2018), I found 103 algos, 65 more than 9 years ago (based on this comment: https://secure.php.net/manual/en/function.hash.php#89574 )

Also, here is my results: https://ideone.com/embed/0iwuGn

I&apos;m not posting here due to message length limitations.

  

#



The well known hash functions MD5 and SHA1 should be avoided in new applications. Collission attacks against MD5 are well documented in the cryptographics literature and have already been demonstrated in practice. Therefore, MD5 is no longer secure for certain applications.

Collission attacks against SHA1 have also been published, though they still require computing power, which is somewhat out of scope. As computing power increases with time and the attacks are likely to get better, too, attacks against systems relying on SHA1 for security are likely to become feasible within the next few years.

There is no lack of potential alternative hash algorithms, as the many choices for the &quot;algo&quot; argument of PHPs hash() function already suggests. Unfortunately, there is lack of analysis, as to how secure these alternative algorithms are. It is rather safe to assume, though, that the SHA2 family with its most prominent members SHA-256 und SHA-512, is better than SHA1.

When storing password hashes, it is a good idea to prefix a salt to the password before hashing, to avoid the same passwords to hash to the same values and to avoid the use of rainbow tables for password recovery. Unlike suggested in other articles, there is no security advantage in putting the salt in the middle, or even at both the beginning and the end, of the combined salt-password-string.

Rather, there are two other factors, that determine the strength of the salt: Its length and its variability. For example, using the same salt for all passwords is easy to implement, but gives only very little additional security. In particular, if users type the same passwords, they will still hash to the same value!

Therefore, the salt should be random string with at least as many variable bits, as there are bits in the hash result. In the user database, store username, the randomly generated salt for that user, and the result of hashing the salt-password-string. Access authentication is then done by looking up the entry for the user, calculating the hash of the salt found in the database and the password provided by the user, and comparing the result with the one stored in the database.

  

#



Performance test results on my laptop:
Results are here shorten to fit php web notes ...
This was tested with 1024000 bytes (1000 KB) of random data, md4 always gets the first place, and md2 always get the last place :)

Results: (in microseconds)
&#xA0;&#xA0; 1.&#xA0; md4&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 5307.912
&#xA0;&#xA0; 2.&#xA0; md5&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 6890.058
&#xA0;&#xA0; 3.&#xA0; crc32b&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 7298.946
&#xA0;&#xA0; 4.&#xA0; crc32&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 7561.922
&#xA0;&#xA0; 5.&#xA0; sha1&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 8886.098
&#xA0;&#xA0; 6.&#xA0; tiger128,3&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 11054.992
&#xA0;&#xA0; 7.&#xA0; haval192,3&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 11132.955
&#xA0;&#xA0; 8.&#xA0; haval224,3&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 11160.135
&#xA0;&#xA0; 9.&#xA0; tiger160,3&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 11162.996
&#xA0; 10.&#xA0; haval160,3&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 11242.151
&#xA0; 11.&#xA0; haval256,3&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 11327.981
&#xA0; 12.&#xA0; tiger192,3&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 11630.058
&#xA0; 13.&#xA0; haval128,3&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 11880.874
&#xA0; 14.&#xA0; tiger192,4&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 14776.945
&#xA0; 15.&#xA0; tiger128,4&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 14871.12
&#xA0; 16.&#xA0; tiger160,4&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 14946.937
&#xA0; 17.&#xA0; haval160,4&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 15661.954
&#xA0; 18.&#xA0; haval192,4&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 15717.029
&#xA0; 19.&#xA0; haval256,4&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 15759.944
&#xA0; 20.&#xA0; adler32&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 15796.184
&#xA0; 21.&#xA0; haval128,4&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 15887.022
&#xA0; 22.&#xA0; haval224,4&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 16047.954
&#xA0; 23.&#xA0; ripemd256&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 16245.126
&#xA0; 24.&#xA0; haval160,5&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 17818.927
&#xA0; 25.&#xA0; haval128,5&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 17887.115
&#xA0; 26.&#xA0; haval224,5&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 18085.002
&#xA0; 27.&#xA0; haval192,5&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 18135.07
&#xA0; 28.&#xA0; haval256,5&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 18678.903
&#xA0; 29.&#xA0; sha256&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 19020.08
&#xA0; 30.&#xA0; ripemd128&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 20671.844
&#xA0; 31.&#xA0; ripemd160&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 21853.923
&#xA0; 32.&#xA0; ripemd320&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 22425.889
&#xA0; 33.&#xA0; sha384&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 45102.119
&#xA0; 34.&#xA0; sha512&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 45655.965
&#xA0; 35.&#xA0; gost&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 57237.148
&#xA0; 36.&#xA0; whirlpool&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 64682.96
&#xA0; 37.&#xA0; snefru&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 80352.783
&#xA0; 38.&#xA0; md2&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 705397.844

Code for generating this:
(compatible both with browser and cli mode)

&lt;pre&gt;



```
<?php

echo &apos;Building random data ...&apos; . PHP_EOL; 
@ob_flush();flush();

$data = &apos;&apos;;
for ($i = 0; $i &lt; 64000; $i++)
&#xA0; &#xA0; $data .= hash(&apos;md5&apos;, rand(), true);

echo strlen($data) . &apos; bytes of random data built !&apos; . PHP_EOL . PHP_EOL . &apos;Testing hash algorithms ...&apos; . PHP_EOL; 
@ob_flush();flush();

$results = array();
foreach (hash_algos() as $v) {
&#xA0; &#xA0; echo $v . PHP_EOL; 
&#xA0; &#xA0; @ob_flush();flush();
&#xA0; &#xA0; $time = microtime(true);
&#xA0; &#xA0; hash($v, $data, false);
&#xA0; &#xA0; $time = microtime(true) - $time;
&#xA0; &#xA0; $results[$time * 1000000000][] = &quot;$v (hex)&quot;;
&#xA0; &#xA0; $time = microtime(true);
&#xA0; &#xA0; hash($v, $data, true);
&#xA0; &#xA0; $time = microtime(true) - $time;
&#xA0; &#xA0; $results[$time * 1000000000][] = &quot;$v (raw)&quot;;
}

ksort($results);

echo PHP_EOL . PHP_EOL . &apos;Results: &apos; . PHP_EOL; 

$i = 1;
foreach ($results as $k =&gt; $v)
&#xA0; &#xA0; foreach ($v as $k1 =&gt; $v1)
&#xA0; &#xA0; &#xA0; &#xA0; echo &apos; &apos; . str_pad($i++ . &apos;.&apos;, 4, &apos; &apos;, STR_PAD_LEFT) . &apos;&#xA0; &apos; . str_pad($v1, 30, &apos; &apos;) . ($k / 1000) . &apos; microseconds&apos; . PHP_EOL;

?>
```


&lt;/pre&gt;

  

#



Just a quick note about these benchmarks and how you should apply them.

If you are hashing passwords etc for security, speed is not your friend. You should use the slowest method.

Slow to hash means slow to crack and will hopefully make generating things like rainbow tables more trouble than it&apos;s worth.

  

#

[Official documentation page](https://www.php.net/manual/en/function.hash.php)

**[To root](/README.md)**