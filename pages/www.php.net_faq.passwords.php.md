# Safe Password Hashing




<div class="phpcode"><span class="html">
I feel like I should comment some of the clams being posted as replies here.<br><br>For starters, speed IS an issue with MD5 in particular and also SHA1. I&apos;ve written my own MD5 bruteforce application just for the fun of it, and using only my CPU I can easily check a hash against about 200mill. hash per second. The main reason for this speed is that you for most attempts can bypass 19 out of 64 steps in the algorithm. For longer input (&gt; 16 characters) it won&apos;t apply, but I&apos;m sure there&apos;s some ways around it.<br><br>If you search online you&apos;ll see people claiming to be able to check against billions of hashes per second using GPUs. I wouldn&apos;t be surprised if it&apos;s possible to reach 100 billion per second on a single computer alone these days, and it&apos;s only going to get worse. It would require a watt monster with 4 dual high-end GPUs or something, but still possible.<br><br>Here&apos;s why 100 billion per second is an issue:<br>Assume most passwords contain a selection of 96 characters. A password with 8 characters would then have 96^8 = 7,21389578984e+15 combinations.<br>With 100 billion per second it would then take 7,21389578984e+15 / 3600 = ~20 hours to figure out what it actually says. Keep in mind that you&apos;ll need to add the numbers for 1-7 characters as well. 20 hours is not a lot if you want to target a single user. <br><br>So on essence:<br>There&apos;s a reason why newer hash algorithms are specifically designed not to be easily implemented on GPUs.<br><br>Oh, and I can see there&apos;s someone mentioning MD5 and rainbow tables. If you read the numbers here, I hope you realize how incredibly stupid and useless rainbow tables have become in terms of MD5. Unless the input to MD5 is really huge, you&apos;re just not going to be able to compete with GPUs here. By the time a storage media is able to produce far beyond 3TB/s, the CPUs and GPUs will have reached much higher speeds.<br><br>As for SHA1, my belief is that it&apos;s about a third slower than MD5. I can&apos;t verify this myself, but it seems to be the case judging the numbers presented for MD5 and SHA1. The issue with speeds is basically very much the same here as well.<br><br>The moral here:<br>Please do as told. Don&apos;t every use MD5 and SHA1 for hasing passwords ever again. We all know passwords aren&apos;t going to be that long for most people, and that&apos;s a major disadvantage. Adding long salts will help for sure, but unless you want to add some hundred bytes of salt, there&apos;s going to be fast bruteforce applications out there ready to reverse engineer your passwords or your users&apos; passwords.</span>
</div>
  

#


<div class="phpcode"><span class="html">
A great read..<br><br><a href="https://nakedsecurity.sophos.com/2013/11/20/serious-security-how-to-store-your-users-passwords-safely/" rel="nofollow" target="_blank">https://nakedsecurity.sophos.com/2013/11/20/serious-security-how-to-store-your-users-passwords-safely/</a><br><br>Serious Security: How to store your users&#x2019; passwords safely<br><br>In summary, here is our minimum recommendation for safe storage of your users&#x2019; passwords:<br><br>&#xA0; &#xA0; Use a strong random number generator to create a salt of 16 bytes or longer.<br>&#xA0; &#xA0; Feed the salt and the password into the PBKDF2 algorithm.<br>&#xA0; &#xA0; Use HMAC-SHA-256 as the core hash inside PBKDF2.<br>&#xA0; &#xA0; Perform 20,000 iterations or more. (June 2016.)<br>&#xA0; &#xA0; Take 32 bytes (256 bits) of output from PBKDF2 as the final password hash.<br>&#xA0; &#xA0; Store the iteration count, the salt and the final hash in your password database.<br>&#xA0; &#xA0; Increase your iteration count regularly to keep up with faster cracking tools.<br><br>Whatever you do, don&#x2019;t try to knit your own password storage algorithm.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/faq.passwords.php)

**[To root](/README.md)**