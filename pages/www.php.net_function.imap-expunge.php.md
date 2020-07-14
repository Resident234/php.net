# imap_expunge



@eisbrenner at gidn dot de<br><br>You shouldn&apos;t call imap_expunge until before closing the connection. imap_delete tags a message for deletion, imap_expunge deletes all tagged messages. i.e.:<br>for ($i = 0; $i &lt; $num; $i++) {<br>  imap_delete($box, $i);<br>}<br>imap_expunge($box);<br><br>imap_expunge should not be in your inner loop.  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-expunge.php)

**[To root](/README.md)**