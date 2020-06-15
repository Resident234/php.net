# connection_status




<div class="phpcode"><span class="html">
Notice !<br><br>if you running a loop (while, foeach etc..)&#xA0; you have to send something to the browser to check the status.<br><br>Example:<br><br>while(1){<br>&#xA0; &#xA0; if (connection_status()!=0){<br>&#xA0; &#xA0; die;<br>&#xA0; &#xA0; }<br>}<br>doesnt work, if the user break/close the browser.<br><br>But a:<br><br>while(1){<br>&#xA0; &#xA0; Echo &quot;\n&quot;; //&lt;-- send this to the client<br>&#xA0; &#xA0; if (connection_status()!=0){<br>&#xA0; &#xA0; die;<br>&#xA0; &#xA0; }<br>}<br>will work :)<br><br>i hope it will help some of you to safe some time :)<br><br>Toppi</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.connection-status.php)

**[To root](/README.md)**