# Locale::acceptFromHttp



It&apos;s good to mention that if user browser will not send HTTP_ACCEPT_LANGUAGE, the output from:<br><br> Locale::acceptFromHttp($_SERVER[&apos;HTTP_ACCEPT_LANGUAGE&apos;]);<br><br>Will be null. <br><br>So remember to set up a fail over scenario!  

---

[Official documentation page](https://www.php.net/manual/en/locale.acceptfromhttp.php)

**[To root](/README.md)**