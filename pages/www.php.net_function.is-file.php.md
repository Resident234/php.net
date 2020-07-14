# is_file



Note that is_file() returns false if the parent directory doesn&apos;t have +x set for you; this make sense, but other functions such as readdir() don&apos;t seem to have this limitation. The end result is that you can loop through a directory&apos;s files but is_file() will always fail.  

#

[Official documentation page](https://www.php.net/manual/en/function.is-file.php)

**[To root](/README.md)**