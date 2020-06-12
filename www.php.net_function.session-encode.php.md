# session_encode




<div class="phpcode"><span class="html">
session_encode() just return the session dataset in a formatted form<br><br>session_start();<br><br>$_SESSION[&apos;login_ok&apos;] = true;<br>$_SESSION[&apos;nome&apos;] = &apos;sica&apos;;<br>$_SESSION[&apos;inteiro&apos;] = 34;<br><br>echo session_encode();<br><br>this code will print<br><br>login_ok|b:1;nome|s:4:&quot;sica&quot;;inteiro|i:34;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.session-encode.php)

**[⬆ to root](/)**