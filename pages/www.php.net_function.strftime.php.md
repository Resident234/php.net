# strftime



The page mentions that the available conversion specifiers may be different in "Windows, some Linux distributions, and a few other operating systems".<br><br>Specifically, the list of specifiers on this page is basically those from standard GNU/Linux (but without %E, %O, and %+), which is a combination of specifiers from a few different standards. See: http://man7.org/linux/man-pages/man3/strftime.3.html<br><br>This means that, if you&apos;re not using Linux, or your Linux distro has a different C library than glibc, there *will* probably be at least a few differences in the supported specifiers. Windows has the most differences, but I&apos;ve also encountered differences in Alpine Linux (which uses musl intsead of glibc) and BSD.  

#

If strange characters are returned use utf8_encode(strftime()) for UTF-8 characters  

#

[Official documentation page](https://www.php.net/manual/en/function.strftime.php)

**[To root](/README.md)**