# srand



Keep in mind that the Suhosin patch which is installed by default on many PHP-installs such as Debian and DirectAdmin completely disables the srand and mt_srand functions for encryption security reasons. To generate reproducible random numbers from a fixed seed on a Suhosin-hardened server you will need to include your own pseudorandom generator code.  

#

[Official documentation page](https://www.php.net/manual/en/function.srand.php)

**[To root](/README.md)**