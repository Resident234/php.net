# get_cfg_var



get_cfg_var returns the value from php.ini directly,while the ini_get returns   the runtime config value. I have tried it on PHP 5.1.6<br><br>[EDIT by danbrown AT php DOT net: The author of this note means that ini_get() will return values set by ini_set(), .htaccess, a local php.ini file, and other functions at runtime.  Conversely, get_cfg_var() will return strictly the server php.ini.]  

---

[Official documentation page](https://www.php.net/manual/en/function.get-cfg-var.php)

**[To root](/README.md)**