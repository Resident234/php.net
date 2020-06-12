# setlocale




<div class="phpcode"><span class="html">
be careful with the LC_ALL setting, as it may introduce some unwanted conversions. For example, I used <br><br>setlocale (LC_ALL, &quot;Dutch&quot;);<br><br>to get my weekdays in dutch on the page. From that moment on (as I found out many hours later) my floating point values from MYSQL where interpreted as integers because the Dutch locale wants a comma (,) instead of a point (.) before the decimals. I tried printf, number_format, floatval.... all to no avail. 1.50 was always printed as 1.00 :(<br><br>When I set my locale to :<br><br> setlocale (LC_TIME, &quot;Dutch&quot;);<br><br>my weekdays are good now and my floating point values too. <br><br>I hope I can save some people the trouble of figuring this out by themselves.<br><br>Rob</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you are looking for a getlocale() function simply pass 0 (zero) as the second parameter to setlocale().<br><br>Beware though if you use the category LC_ALL and some of the locales differ as a string containing all the locales is returned:<br><br><span class="default">&lt;?php<br></span><span class="keyword">echo </span><span class="default">setlocale</span><span class="keyword">(</span><span class="default">LC_ALL</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">);<br><br></span><span class="comment">// LC_CTYPE=en_US.UTF-8;LC_NUMERIC=C;LC_TIME=C;LC_COLLATE=C;LC_MONETARY=C;LC_MESSAGES=C;LC_PAPER=C;LC_NAME=C;<br>// LC_ADDRESS=C;LC_TELEPHONE=C;LC_MEASUREMENT=C;LC_IDENTIFICATION=C<br><br></span><span class="keyword">echo </span><span class="default">setlocale</span><span class="keyword">(</span><span class="default">LC_CTYPE</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">);<br><br></span><span class="comment">// en_US.UTF-8<br><br></span><span class="default">setlocale</span><span class="keyword">(</span><span class="default">LC_ALL</span><span class="keyword">, </span><span class="string">&quot;en_US.UTF-8&quot;</span><span class="keyword">);<br>echo </span><span class="default">setlocale</span><span class="keyword">(</span><span class="default">LC_ALL</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">);<br><br></span><span class="comment">// en_US.UTF-8<br><br></span><span class="default">?&gt;<br></span><br>If you are looking to store and reset the locales you could do something like this:<br><br><span class="default">&lt;?php<br><br>$originalLocales </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&quot;;&quot;</span><span class="keyword">, </span><span class="default">setlocale</span><span class="keyword">(</span><span class="default">LC_ALL</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">));<br></span><span class="default">setlocale</span><span class="keyword">(</span><span class="default">LC_ALL</span><span class="keyword">, </span><span class="string">&quot;nb_NO.utf8&quot;</span><span class="keyword">);<br><br></span><span class="comment">// Do something<br><br></span><span class="keyword">foreach (</span><span class="default">$originalLocales </span><span class="keyword">as </span><span class="default">$localeSetting</span><span class="keyword">) {<br>&#xA0; if (</span><span class="default">strpos</span><span class="keyword">(</span><span class="default">$localeSetting</span><span class="keyword">, </span><span class="string">&quot;=&quot;</span><span class="keyword">) !== </span><span class="default">false</span><span class="keyword">) {<br>&#xA0; &#xA0; list (</span><span class="default">$category</span><span class="keyword">, </span><span class="default">$locale</span><span class="keyword">) = </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&quot;=&quot;</span><span class="keyword">, </span><span class="default">$localeSetting</span><span class="keyword">);<br>&#xA0; }<br>&#xA0; else {<br>&#xA0; &#xA0; </span><span class="default">$category </span><span class="keyword">= </span><span class="default">LC_ALL</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">$locale&#xA0;&#xA0; </span><span class="keyword">= </span><span class="default">$localeSetting</span><span class="keyword">;<br>&#xA0; }<br>&#xA0; </span><span class="default">setlocale</span><span class="keyword">(</span><span class="default">$category</span><span class="keyword">, </span><span class="default">$locale</span><span class="keyword">); <br>}<br><br></span><span class="default">?&gt;<br></span><br>The above works here (Ubuntu Linux) but as the setlocale() function is just wrapping the equivalent system calls, your mileage may vary on the result.</span>
</div>
  

#


<div class="phpcode"><span class="html">
It took me a while to figure out how to get a Finnish locale correctly set on Ubuntu Server with Apache2 and PHP5.<br><br>At first the output for &quot;locale -a&quot; was this:<br>C<br>en_US.utf8<br>POSIX<br><br>I had to install a finnish language pack with<br>&quot;sudo apt-get install language-pack-fi-base&quot;<br><br>Now the output for &quot;locale -a&quot; is:<br>C<br>en_US.utf8<br>fi_FI.utf8<br>POSIX<br><br>The last thing you need to do after installing the correct language pack is restart Apache with &quot;sudo apache2ctl restart&quot;. The locale &quot;fi_FI.utf8&quot; can then be used in PHP5 after restarting Apache.<br><br>For setting Finnish timezone and locale in PHP use:<br><span class="default">&lt;?php<br>date_default_timezone_set</span><span class="keyword">(</span><span class="string">&apos;Europe/Helsinki&apos;</span><span class="keyword">);<br></span><span class="default">setlocale</span><span class="keyword">(</span><span class="default">LC_ALL</span><span class="keyword">, array(</span><span class="string">&apos;fi_FI.UTF-8&apos;</span><span class="keyword">,</span><span class="string">&apos;fi_FI@euro&apos;</span><span class="keyword">,</span><span class="string">&apos;fi_FI&apos;</span><span class="keyword">,</span><span class="string">&apos;finnish&apos;</span><span class="keyword">));<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
//Fix encoding for russian locale on windows<br>$locale = setlocale(LC_ALL, &apos;ru_RU.CP1251&apos;, &apos;rus_RUS.CP1251&apos;, &apos;Russian_Russia.1251&apos;);<br><br>function strftime_fix($format, $locale, $timestamp = time()){<br>&#xA0; &#xA0; // Fix %e for windows<br>&#xA0; &#xA0; if (strtoupper(substr(PHP_OS, 0, 3)) == &apos;WIN&apos;) {<br>&#xA0; &#xA0; &#xA0; &#xA0; $format = preg_replace(&apos;#(?&lt;!%)((?:%%)*)%e#&apos;, &apos;\1%#d&apos;, $format);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; // convert<br>&#xA0; &#xA0; $date_str = strftime($format, $timestamp);<br>&#xA0; &#xA0; if (stripos($locale, &quot;1251&quot;) !== false) {<br>&#xA0; &#xA0; &#xA0; return iconv(&quot;windows-1251&quot;,&quot;utf-8&quot;, $date_str);<br>&#xA0; &#xA0; } elseif (stripos($locale, &quot;1252&quot;) !== false) {<br>&#xA0; &#xA0; &#xA0; return iconv(&quot;windows-1252&quot;,&quot;utf-8&quot;, $date_str);<br>&#xA0; &#xA0; } else {<br>&#xA0; &#xA0; &#xA0; return $date_str;<br>&#xA0; &#xA0; }<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
!!WARNING!!<br><br>The &quot;locale&quot; always depend on the server configuration.<br><br>i.e.:<br>When trying to use &quot;pt_BR&quot; on some servers you will ALWAYS get false. Even with other languages.<br><br>The locale string need to be supported by the server. Sometimes there are diferents charsets for a language, like &quot;pt_BR.utf-8&quot; and &quot;pt_BR.iso-8859-1&quot;, but there is no support for a _standard_ &quot;pt_BR&quot;.<br><br>This problem occours in Windows platform too. Here you need to call &quot;portuguese&quot; or &quot;spanish&quot; or &quot;german&quot; or...<br><br>Maybe the only way to try to get success calling the function setlocale() is:<br>setlocale(LC_ALL, &quot;pt_BR&quot;, &quot;pt_BR.iso-8859-1&quot;, &quot;pt_BR.utf-8&quot;, &quot;portuguese&quot;, ...);<br><br>But NEVER trust on that when making functions like date conversions or number formating. The best way to make sure you are doing the right thing, is using the default &quot;en_US&quot; or &quot;en_UK&quot;, by not calling the setlocale() function. Or, make sure that your server support the lang you want to use, with some tests.<br><br>Remember that: Using the default locale setings is the best way to &quot;talk&quot; with other applications, like dbs or rpc servers, too.<br><br>[]s<br><br>Pigmeu</span>
</div>
  

#


<div class="phpcode"><span class="html">
Pay attention to the syntax.
<br>- UTF8 without dash (&apos;-&apos;)
<br>- locale.codeset and not locale-codeset.
<br>
<br>Stupid newbie error but worth knowing them when starting with gettext.
<br>
<br><span class="default">&lt;?php
<br>$codeset </span><span class="keyword">= </span><span class="string">&quot;UTF8&quot;</span><span class="keyword">;&#xA0; </span><span class="comment">// warning ! not UTF-8 with dash &apos;-&apos;
<br>&#xA0; &#xA0; &#xA0; &#xA0; 
<br>// for windows compatibility (e.g. xampp) : theses 3 lines are useless for linux systems 
<br>
<br></span><span class="default">putenv</span><span class="keyword">(</span><span class="string">&apos;LANG=&apos;</span><span class="keyword">.</span><span class="default">$lang</span><span class="keyword">.</span><span class="string">&apos;.&apos;</span><span class="keyword">.</span><span class="default">$codeset</span><span class="keyword">);
<br></span><span class="default">putenv</span><span class="keyword">(</span><span class="string">&apos;LANGUAGE=&apos;</span><span class="keyword">.</span><span class="default">$lang</span><span class="keyword">.</span><span class="string">&apos;.&apos;</span><span class="keyword">.</span><span class="default">$codeset</span><span class="keyword">);
<br></span><span class="default">bind_textdomain_codeset</span><span class="keyword">(</span><span class="string">&apos;mydomain&apos;</span><span class="keyword">, </span><span class="default">$codeset</span><span class="keyword">);
<br>
<br></span><span class="comment">// set locale
<br></span><span class="default">bindtextdomain</span><span class="keyword">(</span><span class="string">&apos;mydomain&apos;</span><span class="keyword">, </span><span class="default">ABSPATH</span><span class="keyword">.</span><span class="string">&apos;/locale/&apos;</span><span class="keyword">);
<br></span><span class="default">setlocale</span><span class="keyword">(</span><span class="default">LC_ALL</span><span class="keyword">, </span><span class="default">$lang</span><span class="keyword">.</span><span class="string">&apos;.&apos;</span><span class="keyword">.</span><span class="default">$codeset</span><span class="keyword">);
<br></span><span class="default">textdomain</span><span class="keyword">(</span><span class="string">&apos;mydomain&apos;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>where directory structure of locale is (for example) :
<br>locale/fr_FR/LC_MESSAGES/mydomain.mo
<br>locale/en_US/LC_MESSAGES/mydomain.mo
<br>
<br>and ABSPATH is the absolute path to the locale dir
<br>
<br>further note, under linux systems, it seems to be necessary to create the locale at os level using &apos;locale-gen&apos;.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.setlocale.php)

**[⬆ to root](/)**