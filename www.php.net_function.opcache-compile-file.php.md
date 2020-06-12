# opcache_compile_file




<div class="phpcode"><span class="html">
Be aware that opcache will only compile and cache files older than the script execution start.<br><br>For instance, if you use a script to generate cache files (e.g. you don&apos;t have access to shmop and rely on opcache for in-memory data caching instead), opcache_compile_file will not include the generated file in the cache, because its modification time is after the script start.<br><br>The workaround is to use touch() to set a date before the script execution date, then opcache will compile and cache the generated file.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.opcache-compile-file.php)

**[To root](/)**