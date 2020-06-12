# Magic Quotes




<div class="phpcode"><span class="html">
The very reason magic quotes are deprecated is that a one-size-fits-all approach to escaping/quoting is wrongheaded and downright dangerous.&#xA0; Different types of content have different special chars and different ways of escaping them, and what works in one tends to have side effects elsewhere.&#xA0; Any sample code, here or anywhere else, that pretends to work like magic quotes --or does a similar conversion for HTML, SQL, or anything else for that matter -- is similarly wrongheaded and similarly dangerous.<br><br>Magic quotes are not for security.&#xA0; They never have been.&#xA0; It&apos;s a convenience thing -- they exist so a PHP noob can fumble along and eventually write some mysql queries that kinda work, without having to learn about escaping/quoting data properly.&#xA0; They prevent a few accidental syntax errors, as is their job.&#xA0; But they won&apos;t stop a malicious and semi-knowledgeable attacker from trashing the PHP noob&apos;s database.&#xA0; And that poor noob may never even know how or why his database is now gone, because magic quotes (or his spiffy &quot;i&apos;m gonna escape everything&quot; function) gave him a false sense of security.&#xA0; He never had to learn how to really handle untrusted input.<br><br>Data should be escaped where you need it escaped, and for the domain in which it will be used.&#xA0; (mysql_real_escape_string -- NOT addslashes! -- for MySQL (and that&apos;s only unless you have a clue and use prepared statements), htmlentities or htmlspecialchars for HTML, etc.)&#xA0; Anything else is doomed to failure.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/security.magicquotes.php)

**[⬆ to root](/)**