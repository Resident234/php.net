# realpath_cache_size





&quot;realpath_cache_size&quot; is used by PHP to cache the real file system paths of filenames referenced instead of looking them up each time.&#xA0; Every time you perform any of the various file functions or include/require a file and use a relative path, PHP has to look up where that file really exists. PHP caches those values so it doesn&#x2019;t have to search the current working directory and include_path for the file you are working on.

If your website uses lots of relative path files, think about increasing this value.&#xA0; What value is required can be better estimated after monitoring by how fast the cache fills using realpath_cache_size() after restarting. Its contents can be viewed using realpath_cache_get().

  

#

[Official documentation page](https://www.php.net/manual/en/function.realpath-cache-size.php)

**[To root](/README.md)**