# Installation



Having recently installed Apache2.2 with PHP 5.2.17 on my Windows 7 development machine, I want to pass along my findings about how to set things up to load the correct versions of the OpenSSL DLLs. Many people have posted elsewhere about the "DLL Hell" that results if the a wrong version is loaded.<br><br>First, install Apache 2.2 and check its operation, then download the Windows binaries for PHP from http://windows.php.net/download/. Note that according to the sidebar on that page the recommended version of PHP for use with Apache2 is currently 5.2.17, even though it is back level. Plus, this version comes with all the DLLs you need to use OpenSSL -- no need to recompile as the old PHP man page suggests.<br><br>Having verified the PHP installation, turn on the OpenSSL support by uncommenting the line<br><br>extension

```
<?php_openssl.dll

in php.ini, which you will find in the PHP directory (I&apos;ll assume you made that c:/PHP). Next check the location of php_openssl.dll, which you should find in c:/PHP/ext. Also in php.ini find the key extension_dir, and change its value to c:/php/ext. Next, put this location on the end of your PATH (there&apos;s no need to reboot).

At this point, when you start Apache it will attempt to load php_openssl.dll, but if your setup is anything like mine you will see an error. I prefer to start Apache manually, and the error appears in a dialog box: "The ordinal 4114 could not be located in the dynamic link library LIBEAY32.dll". (I&apos;m not sure whether you would get this message if you started Apache as a service). The Apache log also contains an error message saying that php_openssl.dll cannot be loaded, though that message doesn&apos;t name libeay32.dll. Welcome to DLL Hell.

Libeay32.dll enters the picture because php_openssl.dll depends on it (and also on ssleay32.dll). What I think happens is that Apache first tries to load php_openssl.dll programmatically from the path specified by the extension_dir key. But then, the loading of the so-called dependent DLLs is left to Windows&apos; default mechanism. If Windows finds an incompatible version of a dependent DLL, you get the error.

So clearly the fix is to ensure that the correct version of libeay32.dll is loaded. On my machine, at least three other processes have loaded various versions of this same DLL. They include the Mozy backup client, Windows Explorer (because Mozy installs support in Explorer) and the OpenOffice suite. My machine is quite different in this respect from a dedicated server on which one probably wants as few extraneous processes as possible.  Presumably on a server one can follow advice that suggests copying the dlls to the system32 directory, for example. But I&apos;m not about to mess with my other programs by making system-wide changes.

So what to do? I didn&apos;t find the available information on how Windows searches for DLLs to be very useful, mainly because I didn&apos;t understand it. But it does say that the first place Windows looks is "The directory from which the application loaded." 

To cut to the chase, after a lot of experimentation I came to a key realization -- "the application" is APACHE, not PHP. So I copied libeay32.dll to the Apache2.2/bin directory. Problem solved. No error messages and running phpinfo confirms that OpenSSL is present and enabled.

Good luck, and stay out of DLL Hell.?>
```
  

#

Beginning with version 1.1.0 OpenSSL did change their libary names!<br>libeay32.dll is now libcrypto-*.dll (e.g. libcrypto-1_1-x64.dll for OpenSSL 1.1.x on 64bit windows)<br>ssleay32.dll is now libssl-*.dll (e.g. libssl-1_1-x64.dll for OpenSSL 1.1.x on 64bit windows)  

#

[Official documentation page](https://www.php.net/manual/en/openssl.installation.php)

**[To root](/README.md)**