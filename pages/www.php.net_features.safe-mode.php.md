# Safe Mode



Note that safe mode is largely useless. Most ISPs that offer Perl also offer other scripting languages (mostly Perl), and these other languages don&apos;t have the equivalent of PHP.<br>In other words, if PHP&apos;s safe mode won&apos;t allow vandals into your web presence, they will simply use Perl.<br>Also, safe mode prevents scripts from creating and using directories (because they will be owned by the WWW server, not by the user who uploaded the PHP script). So it&apos;s not only useless, it&apos;s also a hindrance.<br>The only realistic option is to bugger the Apache folks until they run all scripts as the user who&apos;s responsible for a given virtualhost or directory.  

---

zebz: The user would not be able to create a directory outside the namespace where he/she would be able to modify its contents. One can&apos;t create a directory that becomes apache-owned unless one owns the parent directory.<br><br>Another security risk: since files created by apache are owned by apache, a user could call the fputs function and output PHP code to a newly-created file with a .php extension, thus creating an apache-owned PHP script on the server. Executing that apache-owned script would allow the script to work with files in the apache user&apos;s namespace, such as logs. A solution would be to force PHP-created files to be owned by the same owner/group as the script that created them. Using open_basedir would be a likely workaround to prevent ascension into uncontrolled areas.  

---

readfile() seems not always to take care of the safe_mode setting.<br>When the file you want to read with readfile() resides in the same directory as the script itself, it doesn`t matter who the owner of the file is, readfile() will work.  

---

[Official documentation page](https://www.php.net/manual/en/features.safe-mode.php)

**[To root](/README.md)**