# gzcompress



Did some simple benchmarking of gzcompress() this morning on a lightly-loaded Fedora 12 server with an AMD Phenom II 940 (quad-core) rocking 8 gigs of ram and executing PHP 5.3.2.2 as an Apache module:<br><br>Compression benchmark: (level, time, size (%)):<br>0: 0.000373 - 82.08 kB (100.02%)<br>1: 0.000914 - 19.61 kB (23.90%)<br>2: 0.000951 - 18.88 kB (23.01%)<br>3: 0.000999 - 18.43 kB (22.46%)<br>4: 0.001498 - 17.65 kB (21.51%)<br>5: 0.001744 - 17.09 kB (20.82%)<br>6: 0.002060 - 16.88 kB (20.57%)<br>7: 0.002233 - 16.85 kB (20.53%)<br>8: 0.002808 - 16.71 kB (20.36%)<br>9: 0.002928 - 16.71 kB (20.36%)<br><br>The time code evaluates to:<br>1. Get start microtime<br>2. Call gzcompress<br>3. Get end microtime<br>4. Report average duration of 100 cycles<br><br>The 82.08 kB was a copied and pasted string of two actual, PHP-generated pages we use on our intranet. Running some rough calculations, the time it takes to compress the data at level 9 will never be larger than the time it takes to transmit slightly-less compressed data at levels 6 or lower. In other words, in our application, we have no reason not to fully-compress each PHP script&apos;s output.  

---

[Official documentation page](https://www.php.net/manual/en/function.gzcompress.php)

**[To root](/README.md)**