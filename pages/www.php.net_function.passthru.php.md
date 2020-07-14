# passthru



Regarding swbrown&apos;s comment...you need to use an output buffer if you don&apos;t want the data displayed.<br><br>For example:<br>ob_start();<br>passthru("&lt;i&gt;command&lt;/i&gt;");<br>$var = ob_get_contents();<br>ob_end_clean(); //Use this instead of ob_flush()<br><br>This gets all the output from the command, and exits without sending any data to stdout.  

#

[Official documentation page](https://www.php.net/manual/en/function.passthru.php)

**[To root](/README.md)**