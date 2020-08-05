# iptcparse



Just to add to the above response, he missed a couple of IPTC tags:<br><br>Keywords:<br>$iptc["2#025"][n];   (there is a list of keywords)<br><br>Caption Writer:<br>$iptc["2#122"][0];<br><br>Just figured I&apos;d note it, as the keywords can be quite important for database applications.  I got these by extracting IPTC tags from a Photoshop 6.0 file, so hopefully they are standardized ;)  

---

[Official documentation page](https://www.php.net/manual/en/function.iptcparse.php)

**[To root](/README.md)**