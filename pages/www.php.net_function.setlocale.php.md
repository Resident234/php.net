# setlocale



be careful with the LC_ALL setting, as it may introduce some unwanted conversions. For example, I used <br><br>setlocale (LC_ALL, "Dutch");<br><br>to get my weekdays in dutch on the page. From that moment on (as I found out many hours later) my floating point values from MYSQL where interpreted as integers because the Dutch locale wants a comma (,) instead of a point (.) before the decimals. I tried printf, number_format, floatval.... all to no avail. 1.50 was always printed as 1.00 :(<br><br>When I set my locale to :<br><br> setlocale (LC_TIME, "Dutch");<br><br>my weekdays are good now and my floating point values too. <br><br>I hope I can save some people the trouble of figuring this out by themselves.<br><br>Rob  

#

If you are looking for a getlocale() function simply pass 0 (zero) as the second parameter to setlocale().<br><br>Beware though if you use the category LC_ALL and some of the locales differ as a string containing all the locales is returned:<br><br>

```
<?php
echo setlocale(LC_ALL, 0);

// LC_CTYPE=en_US.UTF-8;LC_NUMERIC=C;LC_TIME=C;LC_COLLATE=C;LC_MONETARY=C;LC_MESSAGES=C;LC_PAPER=C;LC_NAME=C;
// LC_ADDRESS=C;LC_TELEPHONE=C;LC_MEASUREMENT=C;LC_IDENTIFICATION=C

echo setlocale(LC_CTYPE, 0);

// en_US.UTF-8

setlocale(LC_ALL, "en_US.UTF-8");
echo setlocale(LC_ALL, 0);

// en_US.UTF-8

?>
```


If you are looking to store and reset the locales you could do something like this:



```
<?php

$originalLocales = explode(";", setlocale(LC_ALL, 0));
setlocale(LC_ALL, "nb_NO.utf8");

// Do something

foreach ($originalLocales as $localeSetting) {
  if (strpos($localeSetting, "=") !== false) {
    list ($category, $locale) = explode("=", $localeSetting);
  }
  else {
    $category = LC_ALL;
    $locale   = $localeSetting;
  }
  setlocale($category, $locale); 
}

?>
```
<br><br>The above works here (Ubuntu Linux) but as the setlocale() function is just wrapping the equivalent system calls, your mileage may vary on the result.  

#

It took me a while to figure out how to get a Finnish locale correctly set on Ubuntu Server with Apache2 and PHP5.<br><br>At first the output for "locale -a" was this:<br>C<br>en_US.utf8<br>POSIX<br><br>I had to install a finnish language pack with<br>"sudo apt-get install language-pack-fi-base"<br><br>Now the output for "locale -a" is:<br>C<br>en_US.utf8<br>fi_FI.utf8<br>POSIX<br><br>The last thing you need to do after installing the correct language pack is restart Apache with "sudo apache2ctl restart". The locale "fi_FI.utf8" can then be used in PHP5 after restarting Apache.<br><br>For setting Finnish timezone and locale in PHP use:<br>

```
<?php
date_default_timezone_set(&apos;Europe/Helsinki&apos;);
setlocale(LC_ALL, array(&apos;fi_FI.UTF-8&apos;,&apos;fi_FI@euro&apos;,&apos;fi_FI&apos;,&apos;finnish&apos;));
?>
```
  

#

//Fix encoding for russian locale on windows<br>$locale = setlocale(LC_ALL, &apos;ru_RU.CP1251&apos;, &apos;rus_RUS.CP1251&apos;, &apos;Russian_Russia.1251&apos;);<br><br>function strftime_fix($format, $locale, $timestamp = time()){<br>    // Fix %e for windows<br>    if (strtoupper(substr(PHP_OS, 0, 3)) == &apos;WIN&apos;) {<br>        $format = preg_replace(&apos;#(?&lt;!%)((?:%%)*)%e#&apos;, &apos;\1%#d&apos;, $format);<br>    }<br>    // convert<br>    $date_str = strftime($format, $timestamp);<br>    if (stripos($locale, "1251") !== false) {<br>      return iconv("windows-1251","utf-8", $date_str);<br>    } elseif (stripos($locale, "1252") !== false) {<br>      return iconv("windows-1252","utf-8", $date_str);<br>    } else {<br>      return $date_str;<br>    }<br>}  

#

!!WARNING!!<br><br>The "locale" always depend on the server configuration.<br><br>i.e.:<br>When trying to use "pt_BR" on some servers you will ALWAYS get false. Even with other languages.<br><br>The locale string need to be supported by the server. Sometimes there are diferents charsets for a language, like "pt_BR.utf-8" and "pt_BR.iso-8859-1", but there is no support for a _standard_ "pt_BR".<br><br>This problem occours in Windows platform too. Here you need to call "portuguese" or "spanish" or "german" or...<br><br>Maybe the only way to try to get success calling the function setlocale() is:<br>setlocale(LC_ALL, "pt_BR", "pt_BR.iso-8859-1", "pt_BR.utf-8", "portuguese", ...);<br><br>But NEVER trust on that when making functions like date conversions or number formating. The best way to make sure you are doing the right thing, is using the default "en_US" or "en_UK", by not calling the setlocale() function. Or, make sure that your server support the lang you want to use, with some tests.<br><br>Remember that: Using the default locale setings is the best way to "talk" with other applications, like dbs or rpc servers, too.<br><br>[]s<br><br>Pigmeu  

#

Pay attention to the syntax.<br>- UTF8 without dash (&apos;-&apos;)<br>- locale.codeset and not locale-codeset.<br><br>Stupid newbie error but worth knowing them when starting with gettext.<br><br>

```
<?php
$codeset = "UTF8";  // warning ! not UTF-8 with dash &apos;-&apos;
        
// for windows compatibility (e.g. xampp) : theses 3 lines are useless for linux systems 

putenv(&apos;LANG=&apos;.$lang.&apos;.&apos;.$codeset);
putenv(&apos;LANGUAGE=&apos;.$lang.&apos;.&apos;.$codeset);
bind_textdomain_codeset(&apos;mydomain&apos;, $codeset);

// set locale
bindtextdomain(&apos;mydomain&apos;, ABSPATH.&apos;/locale/&apos;);
setlocale(LC_ALL, $lang.&apos;.&apos;.$codeset);
textdomain(&apos;mydomain&apos;);
?>
```
<br><br>where directory structure of locale is (for example) :<br>locale/fr_FR/LC_MESSAGES/mydomain.mo<br>locale/en_US/LC_MESSAGES/mydomain.mo<br><br>and ABSPATH is the absolute path to the locale dir<br><br>further note, under linux systems, it seems to be necessary to create the locale at os level using &apos;locale-gen&apos;.  

#

[Official documentation page](https://www.php.net/manual/en/function.setlocale.php)

**[To root](/README.md)**