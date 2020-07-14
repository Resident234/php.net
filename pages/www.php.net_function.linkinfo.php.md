# linkinfo



I expected this function to return FALSE or 0 if a symbolic link did not exist (per the documentation above), but that&apos;s not what happened. Reading the man page for the Linux kerne&apos;s stat call here: http://www.kernel.org/doc/man-pages/online/pages/man2/stat.2.html it says this:<br><br>RETURN VALUE - On success, zero is returned.  On error, -1 is returned, and errno is set appropriately.<br><br>... which is what is happening in my case. I am doing a linkinfo(&apos;/path/to/file&apos;); on a missing symlink, and I get back a value of -1. As we know, a value of -1 is not going to evaluate to a FALSE or 0.<br><br>My point - be careful with return values for missing symlinks.  

#

[Official documentation page](https://www.php.net/manual/en/function.linkinfo.php)

**[To root](/README.md)**