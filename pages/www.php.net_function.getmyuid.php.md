# getmyuid



Note that this function really does what the description says, it returns the numeric user id of the user who *owns the file* containing the current script not the effective user id of user *running* the current script.  Most applications will want the latter which is provided by posix_getuid().  

---

[Official documentation page](https://www.php.net/manual/en/function.getmyuid.php)

**[To root](/README.md)**