# sha1



The suggestion below to double-hash your password is not a good idea.  You are much much better off adding a variable salt to passwords before hashing (such as the username or other field that is dissimilar for every account).<br><br>Double hashing is *worse* security than a regular hash.  What you&apos;re actually doing is taking some input $passwd, converting it to a string of exactly 32 characters containing only the characters [0-9][A-F], and then hashing *that*. You have just *greatly* increased the odds of a hash collision (ie. the odds that I can guess a phrase that will hash to the same value as your password).<br><br>sha1(md5($pass)) makes even less sense, since you&apos;re feeding in 128-bits of information to generate a 256-bit hash, so 50% of the resulting data is redundant.  You have not increased security at all.  

---

[Official documentation page](https://www.php.net/manual/en/function.sha1.php)

**[To root](/README.md)**