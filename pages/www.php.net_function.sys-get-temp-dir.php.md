# sys_get_temp_dir



As of PHP 5.5.0, you can set the sys_temp_dir INI setting so that this function will return a useful value when the default temporary directory is not an option.  

---

If running on a Linux system where systemd has PrivateTmp=true (which is the default on CentOS 7 and perhaps other newer distros), this function will simply return "/tmp", not the true, much longer, somewhat dynamic path.  

---

This function does not always add trailing slash. This behaviour is inconsistent across systems, so you have keep an eye on it.  

---

[Official documentation page](https://www.php.net/manual/en/function.sys-get-temp-dir.php)

**[To root](/README.md)**