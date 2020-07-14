# The FilesystemIterator class



You may be wondering, like I did, what is the difference between this class and DirectoryIterator?<br><br>When you iteterate using DirectoryIterator each "value" returned is the same DirectoryIterator object. The internal state is changed so when you call isDir(), getPathname(), etc the correct information is returned. If you were to ask for a key when iterating you will get an integer index value.<br><br>FilesystemIterator (and RecursiveDirectoryIterator) on the other hand returns a new, different SplFileInfo object for each iteration step. The key is the full pathname of the file. This is by default. You can change what is returned for the key or value using the "flags" arguement to the constructor.  

#

[Official documentation page](https://www.php.net/manual/en/class.filesystemiterator.php)

**[To root](/README.md)**