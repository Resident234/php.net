# gzcompress





Did some simple benchmarking of gzcompress() this morning on a lightly-loaded Fedora 12 server with an AMD Phenom II 940 (quad-core) rocking 8 gigs of ram and executing PHP 5.3.2.2 as an Apache module:

Compression benchmark: (level, time, size (%)):
0: 0.000373 - 82.08 kB (100.02%)
1: 0.000914 - 19.61 kB (23.90%)
2: 0.000951 - 18.88 kB (23.01%)
3: 0.000999 - 18.43 kB (22.46%)
4: 0.001498 - 17.65 kB (21.51%)
5: 0.001744 - 17.09 kB (20.82%)
6: 0.002060 - 16.88 kB (20.57%)
7: 0.002233 - 16.85 kB (20.53%)
8: 0.002808 - 16.71 kB (20.36%)
9: 0.002928 - 16.71 kB (20.36%)

The time code evaluates to:
1. Get start microtime
2. Call gzcompress
3. Get end microtime
4. Report average duration of 100 cycles

The 82.08 kB was a copied and pasted string of two actual, PHP-generated pages we use on our intranet. Running some rough calculations, the time it takes to compress the data at level 9 will never be larger than the time it takes to transmit slightly-less compressed data at levels 6 or lower. In other words, in our application, we have no reason not to fully-compress each PHP script&apos;s output.

  

#

[Official documentation page](https://www.php.net/manual/en/function.gzcompress.php)

**[To root](/README.md)**