# spl_object_hash



Note that the contents (properties) of the object are NOT hashed by the function, merely its internal handle and handler table pointer. This is sufficient to guarantee that any two objects simultaneously co-residing in memory will have different hashes. Uniqueness is not guaranteed between objects that did not reside in memory simultaneously, for example:<br><br>var_dump(spl_object_hash(new stdClass()), spl_object_hash(new stdClass()));<br><br>Running this alone will usually generate the same hashes, since PHP reuses the internal handle for the first stdClass after it has been dereferenced and destroyed when it creates the second stdClass.  

#

Note that given two different objects spl_object_hash() can return values that look very similar, and in fact both the most significant *and* least significant digits are likely to be identical!  e.g. "000000003cc56d770000000007fa48c5" and "000000003cc56d0d0000000007fa48c5".<br><br>Therefore (especially if using this function for debugging), you may wish to pass the hash into a cryptographic hash function like md5() to get to facilitate visual comparisons, and make it more likely that the first few or last few digits are unique.<br><br>md5("000000003cc56d770000000007fa48c5") -&gt; "619a799747d348fa1caf181a72b65d9f"<br><br>md5("000000003cc56d0d0000000007fa48c5") -&gt; "ae964bc912281e7804fe5a88b4546cb2"  

#

Please note that since PHP 7.2 there&apos;s new function available spl_object_id() which returns int instead of string. It&apos;s (supposed to be) more performant. Due to lack of documentation I recommend you reading the PR https://github.com/php/?>
```
src/pull/2611  

#

[Official documentation page](https://www.php.net/manual/en/function.spl-object-hash.php)

**[To root](/README.md)**