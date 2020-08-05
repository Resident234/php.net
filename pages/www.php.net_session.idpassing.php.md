# Passing the Session ID



The first time a page is accessed, PHP doesn&apos;t yet know if the browser is accepting cookies or not, so after session_start() is called, SID will be non-empty, and the PHPSESSID gets inserted in all link URLs on that page that are properly using SID.<br><br>This has the consequence that if, for example, a search engine bot hits your home page first, all links that it sees on your home page will have the ugly PHPSESSID=... in them.<br><br>This appears to be the default behavior. A work-around is to turn on session.use_only_cookies, but then you lose session data for anyone who has their cookies turned off.  

---

[Official documentation page](https://www.php.net/manual/en/session.idpassing.php)

**[To root](/README.md)**