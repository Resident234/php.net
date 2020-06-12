# setcookie




<div class="phpcode"><span class="html">
Instead of this:
<br><span class="default">&lt;?php setcookie</span><span class="keyword">( </span><span class="string">&quot;TestCookie&quot;</span><span class="keyword">, </span><span class="default">$value</span><span class="keyword">, </span><span class="default">time</span><span class="keyword">()+(</span><span class="default">60</span><span class="keyword">*</span><span class="default">60</span><span class="keyword">*</span><span class="default">24</span><span class="keyword">*</span><span class="default">30</span><span class="keyword">) ); </span><span class="default">?&gt;
<br></span>
<br>You can this:
<br><span class="default">&lt;?php setcookie</span><span class="keyword">( </span><span class="string">&quot;TestCookie&quot;</span><span class="keyword">, </span><span class="default">$value</span><span class="keyword">, </span><span class="default">strtotime</span><span class="keyword">( </span><span class="string">&apos;+30 days&apos; </span><span class="keyword">) ); </span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Want to remove a cookie?<br><br>Many people do it the complicated way:<br>setcookie(&apos;name&apos;, &apos;content&apos;, time()-3600);<br><br>But why do you make it so complicated and risk it not working, when the client&apos;s time is wrong? Why fiddle around with time();<br><br>Here&apos;s the easiest way to unset a cookie:<br>setcookie(&apos;name&apos;, &apos;content&apos;, 1);<br><br>Thats it.</span>
</div>
  

#


<div class="phpcode"><span class="html">
if you are having problems seeing cookies sometimes or deleting cookies sometimes, despite following the advice below, make sure you are setting the cookie with the domain argument. Set it with the dot before the domain as the examples show: &quot;.example.com&quot;.&#xA0; I wasn&apos;t specifying the domain, and finally realized I was setting the cookie when the browser url had the <a href="http://www.example.com" rel="nofollow" target="_blank">http://www.example.com</a> and later trying to delete it when the url didn&apos;t have the www. ie. <a href="http://example.com." rel="nofollow" target="_blank">http://example.com.</a> This also caused the page to be unable to find the cookie when the www. wasn&apos;t in the domain.&#xA0; (When you add the domain argument to the setcookie code that creates the cookie, make sure you also add it to the code that deletes the cookie.)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note when setting &quot;array cookies&quot; that a separate cookie is set for each element of the array.<br><br>On high traffic sites, this can substantially increase the size of subsequent HTTP requests from clients (including requests for static content on the same domain).<br><br>More importantly though, the cookie specification says that browsers need only accept 20 cookies per domain.&#xA0; This limit is increased to 50 by Firefox, and to 30 by Opera, but IE6 and IE7 enforce the limit of 20 cookie per domain.&#xA0; Any cookies beyond this limit will either knock out an older cookie or be ignored/rejected by the browser.</span>
</div>
  

#


<div class="phpcode"><span class="html">
something that wasn&apos;t made clear to me here and totally confused me for a while was that domain names must contain at least two dots (.), hence &apos;localhost&apos; is invalid and the browser will refuse to set the cookie! instead for localhost you should use false.<br><br>to make your code work on both localhost and a proper domain, you can do this:<br><br><span class="default">&lt;?php<br><br>$domain </span><span class="keyword">= (</span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;HTTP_HOST&apos;</span><span class="keyword">] != </span><span class="string">&apos;localhost&apos;</span><span class="keyword">) ? </span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;HTTP_HOST&apos;</span><span class="keyword">] : </span><span class="default">false</span><span class="keyword">;<br></span><span class="default">setcookie</span><span class="keyword">(</span><span class="string">&apos;cookiename&apos;</span><span class="keyword">, </span><span class="string">&apos;data&apos;</span><span class="keyword">, </span><span class="default">time</span><span class="keyword">()+</span><span class="default">60</span><span class="keyword">*</span><span class="default">60</span><span class="keyword">*</span><span class="default">24</span><span class="keyword">*</span><span class="default">365</span><span class="keyword">, </span><span class="string">&apos;/&apos;</span><span class="keyword">, </span><span class="default">$domain</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">);<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It&apos;s worth a mention: you should avoid dots on cookie names.<br><br><span class="default">&lt;?php<br></span><span class="comment">// this will actually set &apos;ace_fontSize&apos; name:<br></span><span class="default">setcookie</span><span class="keyword">( </span><span class="string">&apos;ace.fontSize&apos;</span><span class="keyword">, </span><span class="default">18 </span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you&apos;re having problem with IE not accepting session cookies this could help:<br><br>It seems the IE (6, 7, 8 and 9) do not accept the part &apos;Expire=0&apos; when setting a session cookie. To fix it just don&apos;t put any expire at all. The default behavior when the &apos;Expire&apos; is not set is to set the cookie as a session one. <br><br>(Firefox doesn&apos;t complains, btw.)</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to delete all cookies on your domain, you may want to use the value of:<br><br><span class="default">&lt;?php $_SERVER</span><span class="keyword">[</span><span class="string">&apos;HTTP_COOKIE&apos;</span><span class="keyword">] </span><span class="default">?&gt;<br></span><br>rather than:<br><br><span class="default">&lt;?php $_COOKIE ?&gt;<br></span><br>to dertermine the cookie names. <br>If cookie names are in Array notation, eg: user[username] <br>Then PHP will automatically create a corresponding array in $_COOKIE. Instead use $_SERVER[&apos;HTTP_COOKIE&apos;] as it mirrors the actual HTTP Request header. <br><br><span class="default">&lt;?php<br><br></span><span class="comment">// unset cookies<br></span><span class="keyword">if (isset(</span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;HTTP_COOKIE&apos;</span><span class="keyword">])) {<br>&#xA0; &#xA0; </span><span class="default">$cookies </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos;;&apos;</span><span class="keyword">, </span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;HTTP_COOKIE&apos;</span><span class="keyword">]);<br>&#xA0; &#xA0; foreach(</span><span class="default">$cookies </span><span class="keyword">as </span><span class="default">$cookie</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$parts </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos;=&apos;</span><span class="keyword">, </span><span class="default">$cookie</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$name </span><span class="keyword">= </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$parts</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">setcookie</span><span class="keyword">(</span><span class="default">$name</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="default">time</span><span class="keyword">()-</span><span class="default">1000</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">setcookie</span><span class="keyword">(</span><span class="default">$name</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="default">time</span><span class="keyword">()-</span><span class="default">1000</span><span class="keyword">, </span><span class="string">&apos;/&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
About the delete part, I found that Firefox only remove the cookie when you submit the same values for all parameters, except the date, which sould be in the past. Submiting blank values didn&apos;t work for me.
<br>
<br>Example :
<br>- set -
<br>
<br><span class="default">&lt;?php setcookie</span><span class="keyword">( </span><span class="string">&quot;name&quot;</span><span class="keyword">, </span><span class="string">&quot;value&quot;</span><span class="keyword">, </span><span class="string">&quot;future_timestamp&quot;</span><span class="keyword">, </span><span class="string">&quot;path&quot;</span><span class="keyword">, </span><span class="string">&quot;domain&quot; </span><span class="keyword">); </span><span class="default">?&gt;
<br></span>
<br>- delete -
<br><span class="default">&lt;?php setcookie</span><span class="keyword">( </span><span class="string">&quot;name&quot;</span><span class="keyword">, </span><span class="string">&quot;value&quot;</span><span class="keyword">, </span><span class="string">&quot;past_timestamp&quot;</span><span class="keyword">, </span><span class="string">&quot;path&quot;</span><span class="keyword">, </span><span class="string">&quot;domain&quot; </span><span class="keyword">); </span><span class="default">?&gt;
<br></span>
<br>Jonathan</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.setcookie.php)

**[⬆ to root](/)**