# password_needs_rehash




<div class="phpcode"><span class="html">
nick, this function cannot check if a string is a MD5 or SHA1 hash. It can only tell you if a password, hashed using the password_hash function, needs to be put through the hashing function again to keep up to date with the new defaults.<br><br>The only time you can use this function is when your user logs in and you have already checked by means of password_verify that the password entered is actually correct. At that point, if password_needs_rehash returns true, you can put the plain text password through the password_hash function.</span>
</div>
  

#


<div class="phpcode"><span class="html">
ydroneaud this would be used on a login page, not at any other time.<br><br>So if you have a site with MD5 passwords for example, and wish to upgrade to SHA256 for additional security you would put this check in the login script.<br><br>This function will take a user&apos;s hash and say if it is SHA256, if it isn&apos;t then you can take the user&apos;s password which you still have as plaintext and rehash it as SHA256.<br><br>This lets you gradually update the hashes in your database without disrupting any features or resetting passwords.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Some other use-cases for the password_needs_rehash function is when you have specified using the PASSWORD_DEFAULT algorithm for password_hash.<br>As mentioned on the Password Hashing Predefined Constants and password_hash pages, the algorithm used by PASSWORD_DEFAULT is subject to change as different versions of PHP are released.<br>Additionally password_needs_rehash would be used if you have changed the optional cost or static salt (DO NOT USE A STATIC SALT) requirements of your password_hash options.<br><br>Full example:<br><br><span class="default">&lt;?php<br><br>$new </span><span class="keyword">= [<br>&#xA0; &#xA0; </span><span class="string">&apos;options&apos; </span><span class="keyword">=&gt; [</span><span class="string">&apos;cost&apos; </span><span class="keyword">=&gt; </span><span class="default">11</span><span class="keyword">],<br>&#xA0; &#xA0; </span><span class="string">&apos;algo&apos; </span><span class="keyword">=&gt; </span><span class="default">PASSWORD_DEFAULT</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="string">&apos;hash&apos; </span><span class="keyword">=&gt; </span><span class="default">null<br></span><span class="keyword">];<br><br></span><span class="default">$password </span><span class="keyword">= </span><span class="string">&apos;rasmuslerdorf&apos;</span><span class="keyword">;<br><br></span><span class="comment">//stored hash of password<br></span><span class="default">$oldHash </span><span class="keyword">= </span><span class="string">&apos;$2y$07$BCryptRequires22Chrcte/VlQH0piJtjXl.0t1XkA8pw9dMXTpOq&apos;</span><span class="keyword">;<br><br></span><span class="comment">//verify stored hash against plain-text password<br></span><span class="keyword">if (</span><span class="default">true </span><span class="keyword">=== </span><span class="default">password_verify</span><span class="keyword">(</span><span class="default">$password</span><span class="keyword">, </span><span class="default">$oldHash</span><span class="keyword">)) {<br>&#xA0; &#xA0; </span><span class="comment">//verify legacy password to new password_hash options<br>&#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">true </span><span class="keyword">=== </span><span class="default">password_needs_rehash</span><span class="keyword">(</span><span class="default">$oldHash</span><span class="keyword">, </span><span class="default">$new</span><span class="keyword">[</span><span class="string">&apos;algo&apos;</span><span class="keyword">], </span><span class="default">$new</span><span class="keyword">[</span><span class="string">&apos;options&apos;</span><span class="keyword">])) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//rehash/store plain-text password using new hash<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$newHash </span><span class="keyword">= </span><span class="default">password_hash</span><span class="keyword">(</span><span class="default">$password</span><span class="keyword">, </span><span class="default">$new</span><span class="keyword">[</span><span class="string">&apos;algo&apos;</span><span class="keyword">], </span><span class="default">$new</span><span class="keyword">[</span><span class="string">&apos;options&apos;</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="default">$newHash</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;<br></span><br>The above example will output something similar to:<br>$2y$11$Wu5rN3u38.g/XWdUeA6Wj.PD.F0fLXXmZrMNFyzzg2UxkVmxlk41W</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.password-needs-rehash.php)

**[To root](/README.md)**