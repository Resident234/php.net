# setrawcookie




<div class="phpcode"><span class="html">
Firefox is following the real spec and does not decode &apos;+&apos; to space...in fact it further encodes them to &apos;%2B&apos; to store the cookie.&#xA0; If you read a cookie using javascript and unescape it, all your spaces will be turned to &apos;+&apos;.
<br>To fix this problem, use setrawcookie and rawurlencode:
<br>
<br><span class="default">&lt;?php
<br>setrawcookie</span><span class="keyword">(</span><span class="string">&apos;cookie_name&apos;</span><span class="keyword">, </span><span class="default">rawurlencode</span><span class="keyword">(</span><span class="default">$value</span><span class="keyword">), </span><span class="default">time</span><span class="keyword">()+</span><span class="default">60</span><span class="keyword">*</span><span class="default">60</span><span class="keyword">*</span><span class="default">24</span><span class="keyword">*</span><span class="default">365</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>The only change is that spaces will be encoded to &apos;%20&apos; instead of &apos;+&apos; and will now decode properly.</span>
</div>
  

#


<div class="phpcode"><span class="html">
setrawcookie() isn&apos;t entirely &apos;raw&apos;. It will check the value for invalid characters, and then disallow the cookie if there are any. These are the invalid characters to keep in mind: &apos;,;&lt;space&gt;\t\r\n\013\014&apos;.<br><br>Note that comma, space and tab are three of the invalid characters. IE, Firefox and Opera work fine with these characters, and PHP reads cookies containing them fine as well. However, if you want to use these characters in cookies that you set from php, you need to use header().</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.setrawcookie.php)

**[⬆ to root](/)**