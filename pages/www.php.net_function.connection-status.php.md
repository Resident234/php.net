# connection_status



Notice !<br><br>if you running a loop (while, foeach etc..)  you have to send something to the browser to check the status.<br><br>Example:<br><br>while(1){<br>    if (connection_status()!=0){<br>    die;<br>    }<br>}<br>doesnt work, if the user break/close the browser.<br><br>But a:<br><br>while(1){<br>    Echo "\n"; //&lt;-- send this to the client<br>    if (connection_status()!=0){<br>    die;<br>    }<br>}<br>will work :)<br><br>i hope it will help some of you to safe some time :)<br><br>Toppi  

#

[Official documentation page](https://www.php.net/manual/en/function.connection-status.php)

**[To root](/README.md)**