# Why not to use Magic Quotes




<div class="phpcode"><span class="html">
Additionally, addslashes() is not a cure-all against SQL injection attacks. You should use your database&apos;s dedicated escape function (such as mysql_escape_string) or better yet, use parameterised queries through mysqli-&gt;prepare().</span>
</div>
  

#


<div class="phpcode"><span class="html">
Another reason against it: security. You could be lulled in a feeling of false security if you have magic_quotes=On on a test server and Off on production server. <br><br>And another: readability of the code. If you want to be portable you need to resort to some weird solution, outlines on these pages (if (get_magic_quotes())...).<br><br>Let&apos;s hope magic_quotes soon goes to history together with safe_mode and similar &quot;kind-of-security&quot; (but in reality just a nuisance) inventions.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/security.magicquotes.whynot.php)

**[To root](/)**