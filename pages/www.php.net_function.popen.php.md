# popen





If you try to execute a command under Windows the PHP script normally waits until the process has been terminated. Executing long-term processes pauses a PHP script even if you don&apos;t want to wait for the end of the process.



It wasn&apos;t easy to find this beautiful example how to start a process under Windows without waiting for its termination:





```
<?php

$commandString = &apos;start /b c:\\programToRun.exe -attachment &quot;c:\\temp\file1.txt&quot;&apos;;

pclose(popen($commandString, &apos;r&apos;));

?>
```



  

#



If, on windows, you need to start a batch file that needs administrator privileges, then you can make a shortcut to the batch file, click properties, check to on &quot;run as administrator&quot; on one of the property pages, and then double-click the shortcut once (to initialize that &quot;run as administrator&quot; business).

using popen(&quot;/path/to/shortcut.lnk&quot;) will then run your batch file with administrator privileges.

handy for when you want to use cli php to do some long running tasks and that php-cli needs to use sessions..

  

#

[Official documentation page](https://www.php.net/manual/en/function.popen.php)

**[To root](/README.md)**