# ftp_nlist




<div class="phpcode"><span class="html">
Some complain that ftp_nlist, always return FALSE. I did experience this behavior myself, until I used ftp_pasv, which is useful if your client is behind a firewall (which most clients are now), then ftp_nlist worked just fine. I don&apos;t really know what are all the implications of using ftp_pasv, but if you read or experience that ftp_nlist, ftp_get, ftp_nb_get doesn&apos;t work, try adding the following:<br><br>ftp_pasv($conn_id,true);</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ftp-nlist.php)

**[To root](/README.md)**