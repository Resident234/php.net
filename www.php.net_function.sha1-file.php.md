# sha1_file




<div class="phpcode"><span class="html">
Just for the record - <br><br>As some have pointed out, you have two ways to generate the hash of a file:<br>Method 1 [this function]: sha1_file($file)<br>Method 2: sha1(file_get_contents($file))<br><br>It&apos;s important to realize that these two methods are NOT the same thing. If they were, I seriously doubt this function would exist.<br><br>The key difference, as far as I can tell, is how the file&apos;s contents are loaded. The second method loads the entirety of $file into memory before passing it to sha1($str). Method two, however, loads the contents of $file as they are needed to create the hash.<br><br>If you can guarantee that you&apos;ll only ever have to hash relatively small files, this difference means very little. If you have larger ones, though, loading the entirety of file into memory is a bad idea: best case, you slow down your server as it tries to handle the request; worse case, you run out of memory and don&apos;t get your hash at all.<br><br>Just try to keep this in mind if you decide to load the file&apos;s contents yourself, in lieu of using this function. On my system, I was able to use this function to generate the hash of a 2.6GB file in 22 seconds, whereas I could not with the second method, due to an out-of-memory error (which took 185 seconds).</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.sha1-file.php)

**[⬆ to root](/)**