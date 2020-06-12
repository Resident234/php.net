# mail




<div class="phpcode"><span class="html">
Often it&apos;s helpful to find the exact error message that is triggered by the mail() function. While the function doesn&apos;t provide an error directly, you can use error_get_last() when mail() returns false.<br><br><span class="default">&lt;?php<br>$success </span><span class="keyword">= </span><span class="default">mail</span><span class="keyword">(</span><span class="string">&apos;example@example.com&apos;</span><span class="keyword">, </span><span class="string">&apos;My Subject&apos;</span><span class="keyword">, </span><span class="default">$message</span><span class="keyword">);<br>if (!</span><span class="default">$success</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$errorMessage </span><span class="keyword">= </span><span class="default">error_get_last</span><span class="keyword">()[</span><span class="string">&apos;message&apos;</span><span class="keyword">];<br>}<br></span><span class="default">?&gt;<br></span><br>(Tested successfully on Windows which uses SMTP by default, but sendmail on Linux/OSX may not provide the same level of detail.)<br><br>Thanks to <a href="https://stackoverflow.com/a/20203870/195835" rel="nofollow" target="_blank">https://stackoverflow.com/a/20203870/195835</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
Security advice: Although it is not documented, for the parameters $to and $subject the mail() function changes at least \r and \n to space. So these parameters are safe against injection of additional headers. But you might want to check $to for commas as these separate multiple addresses and you might not want to send to more than one recipient.<br><br>The crucial part is the $additional_headers parameter. This parameter can&apos;t be cleaned by the mail() function. So it is up to you to prevent unwanted \r or \n to be inserted into the values you put in there. Otherwise you just created a potential spam distributor.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.mail.php)

**[⬆ to root](/)**