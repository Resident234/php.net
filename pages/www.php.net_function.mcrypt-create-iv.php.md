# mcrypt_create_iv




<div class="phpcode"><span class="html">
In relation to all of the crypto &quot;advice&quot; seen here, my suggestion is that you ignore most of it. Some of it is good, some of it is bad, but most of it skips the critical issues.<br><br>I had hoped to write out a nice long explanation, but PHP&apos;s commenting system tells me my essay is too long. Instead I will say this:<br><br>You should use CBC, with a randomly chosen IV that is unique per key, and you should transmit that IV in the clear along with your ciphertext. You should also perform an authenticity check of that entire data blob, using something like HMAC-SHA256, with another independent key.<br><br>Here&apos;s the full-text of what I was going to write: <a href="http://pastebin.com/sN6buivY" rel="nofollow" target="_blank">http://pastebin.com/sN6buivY</a><br><br>If you&apos;re interested in this stuff, or just want more information, check out the Wikipedia articles around block cipher modes, block ciphers, HMAC, etc.<br><br>I also suggest reading Practical Cryptography by Bruce Schneier, as well as Cryptography Engineering by Niels Ferguson, both of which are very easy-to-digest books on practical cryptography.</span>
</div>
  

#


<div class="phpcode"><span class="html">
&gt;First, the IV should be random and variable. The whole &gt;point of it is to ensure that the same plaintext does not &gt;encrypt to the same ciphertext every time. You most &gt;certainly do lose security if the IV is constant or public.<br><br>Wrong, Wrong WRONG! The initialization vector is ALLOWED to be PUBLIC! It is generally sent along with the ciphertext, UNENCRYPTED.<br><br>&gt;The ciphertext should be E(IV | plaintext, key)<br><br>Wrong again! The initialization vector is NOT prepended to the plaintext before encryption. The IV is used to seed the feedback system! (which is why you don&apos;t need one in ECB mode - there is no feedback)<br><br>&gt;Second, the IV should not be part of the decryption &gt;parameters at all. You should be able to decrypt the cipher &gt;text, throw away the initial vector at the front w/o even &gt;reading it, and have your plaintext:<br><br>Nope. You need to seed the feedback mechanism during decryption to the SAME state as it was seeded during encryption. This means using the SAME IV!</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.mcrypt-create-iv.php)

**[To root](/README.md)**