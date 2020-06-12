# iptcparse




<div class="phpcode"><span class="html">
Just to add to the above response, he missed a couple of IPTC tags:<br><br>Keywords:<br>$iptc[&quot;2#025&quot;][n];&#xA0;&#xA0; (there is a list of keywords)<br><br>Caption Writer:<br>$iptc[&quot;2#122&quot;][0];<br><br>Just figured I&apos;d note it, as the keywords can be quite important for database applications.&#xA0; I got these by extracting IPTC tags from a Photoshop 6.0 file, so hopefully they are standardized ;)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.iptcparse.php)

**[⬆ to root](/)**