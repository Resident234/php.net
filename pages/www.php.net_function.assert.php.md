# assert



As noted on Wikipedia - "assertions are primarily a development tool, they are often disabled when a program is released to the public." and "Assertions should be used to document logically impossible situations and discover programming errors&#x2014; if the &apos;impossible&apos; occurs, then something fundamental is clearly wrong. This is distinct from error handling: most error conditions are possible, although some may be extremely unlikely to occur in practice. Using assertions as a general-purpose error handling mechanism is usually unwise: assertions do not allow for graceful recovery from errors, and an assertion failure will often halt the program&apos;s execution abruptly. Assertions also do not display a user-friendly error message."<br><br>This means that the advice given by "gk at proliberty dot com" to force assertions to be enabled, even when they have been disabled manually, goes against best practices of only using them as a development tool.  

---

[Official documentation page](https://www.php.net/manual/en/function.assert.php)

**[To root](/README.md)**