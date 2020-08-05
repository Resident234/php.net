# Magic Quotes



The very reason magic quotes are deprecated is that a one-size-fits-all approach to escaping/quoting is wrongheaded and downright dangerous.  Different types of content have different special chars and different ways of escaping them, and what works in one tends to have side effects elsewhere.  Any sample code, here or anywhere else, that pretends to work like magic quotes --or does a similar conversion for HTML, SQL, or anything else for that matter -- is similarly wrongheaded and similarly dangerous.<br><br>Magic quotes are not for security.  They never have been.  It&apos;s a convenience thing -- they exist so a PHP noob can fumble along and eventually write some mysql queries that kinda work, without having to learn about escaping/quoting data properly.  They prevent a few accidental syntax errors, as is their job.  But they won&apos;t stop a malicious and semi-knowledgeable attacker from trashing the PHP noob&apos;s database.  And that poor noob may never even know how or why his database is now gone, because magic quotes (or his spiffy "i&apos;m gonna escape everything" function) gave him a false sense of security.  He never had to learn how to really handle untrusted input.<br><br>Data should be escaped where you need it escaped, and for the domain in which it will be used.  (mysql_real_escape_string -- NOT addslashes! -- for MySQL (and that&apos;s only unless you have a clue and use prepared statements), htmlentities or htmlspecialchars for HTML, etc.)  Anything else is doomed to failure.  

---

[Official documentation page](https://www.php.net/manual/en/security.magicquotes.php)

**[To root](/README.md)**