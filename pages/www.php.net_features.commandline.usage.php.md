# Executing PHP files



On Linux, the shebang (#!) line is parsed by the kernel into at most two parts. <br>For example:<br><br>1:  #!/usr/bin/php<br>2:  #!/usr/bin/env  php<br>3:  #!/usr/bin/php -n<br>4:  #!/usr/bin/php -ddisplay_errors=E_ALL<br>5:  #!/usr/bin/php -n -ddisplay_errors=E_ALL<br><br>1. is the standard way to start a script. (compare "#!/bin/bash".)<br><br>2. uses "env" to find where PHP is installed: it might be elsewhere in the $PATH, such as /usr/local/bin.<br><br>3. if you don&apos;t need to use env, you can pass ONE parameter here. For example, to ignore the system&apos;s PHP.ini, and go with the defaults, use "-n". (See "man php".)<br><br>4.  or, you can set exactly one configuration variable. I recommend this one, because display_errors actually takes effect if it is set here. Otherwise, the only place you can enable it is system-wide in php.ini. If you try to use ini_set() in your script itself, it&apos;s too late: if your script has a parse error, it will silently die. <br><br>5. This will not (as of 2013) work on Linux. It acts as if the whole string, "-n -ddisplay_errors=E_ALL" were a single argument. But in BSD, the shebang line can take more than 2 arguments, and so it may work as intended.<br><br>Summary: use (2) for maximum portability, and (4) for maximum debugging.  

#

[Official documentation page](https://www.php.net/manual/en/features.commandline.usage.php)

**[To root](/README.md)**