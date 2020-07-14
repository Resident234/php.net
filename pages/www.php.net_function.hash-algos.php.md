# hash_algos



Ansewring to holdoffhunger avoid crc32, the are different results because the crc32(); use the algorithm &apos;crc32b&apos;. To check this only have to write:<br><br>echo hash(&apos;crc32b&apos;, &apos;The quick brown fox jumped over the lazy dog.&apos;), "\n";<br>echo dechex (crc32(&apos;The quick brown fox jumped over the lazy dog.&apos;));<br><br>And check that both have the same results:<br><br>82a34642<br>82a34642  

#

[Official documentation page](https://www.php.net/manual/en/function.hash-algos.php)

**[To root](/README.md)**