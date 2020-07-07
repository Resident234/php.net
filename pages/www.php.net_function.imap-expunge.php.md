# imap_expunge





@eisbrenner at gidn dot de

You shouldn&apos;t call imap_expunge until before closing the connection. imap_delete tags a message for deletion, imap_expunge deletes all tagged messages. i.e.:
for ($i = 0; $i &lt; $num; $i++) {
&#xA0; imap_delete($box, $i);
}
imap_expunge($box);

imap_expunge should not be in your inner loop.

  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-expunge.php)

**[To root](/README.md)**