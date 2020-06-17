# Encrypted Storage Model




<div class="phpcode"><span class="html">
Using functions to obfuscate the hash generation does not increase security. This is security by obscurity. The algorithm used to hash the data needs to be secure by itself.<br><br>I would not suggest to use other data as salt. For example if you use the username, you won&apos;t be able to change the values without rehashing the password.<br><br>I would use a dedicated salt value stored in the same database table.<br><br>Why? Because a lot of users use the same login credentials on different web services. And in case another service also uses the username as salt, the resulting hashed password might be the same!<br><br>Also an attacker may prepare a rainbow table with prehashed passwords using the username and other known data as salt. Using random data would easily prevent this with little programming effort.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I would strongly recommend using SHA-2 or better the new SHA-3 hash algorithm. MD5 is practically unusable, since there are very well working rainbow tables around the whole web. Almost the same for SHA-1. Of course you should never do a hash without salting!</span>
</div>
  

#


<div class="phpcode"><span class="html">
A better way to hash would be to use a separate salt for each user. Changing the salt upon each password update will ensure the hashes do not become stale.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/security.database.storage.php)

**[To root](/README.md)**