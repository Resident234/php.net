# ftp_put



ftp_put() overwrites existing files. <br>Again: trivial but not mentioned here.  

---

If when using ftp_put you get the one of the following errors:<br><br>Warning: ftp_put() [function.ftp-put]: Opening ASCII mode data connection<br><br>Warning: ftp_put() [function.ftp-put]: Opening BINARY mode data connection<br><br>and it creates the file in the correct location but is a 0kb file and all FTP commands thereafter fail. It is likely that the client is behind a firewall. To rectify this use:<br><br>

```
<?php
ftp_pasv($resource, true);
?>
```
<br><br>Before executing any put commands. Took me so long to figure this out I actually cheered when I did :D  

---

If you want to copy a whole directory tree (with subdiretories), <br>this function (ftp_copy) might be usefull. Tested with <br>php 4.2.2 and a Linux OS. <br><br>Example:<br>----------------------------------------------------------------<br>$conn_id = ftp_connect("server_adress"); <br>...<br><br>$src_dir = "/from";<br>$dst_dir = "/to";<br><br>ftp_copy($src_dir, $dst_dir);<br>...<br>ftp_close($conn_id)<br><br>Function: ftp_copy()<br>----------------------------------------------------------------<br>function ftp_copy($src_dir, $dst_dir) {<br><br>global $conn_id;<br><br>$d = dir($src_dir);<br><br>    while($file = $d-&gt;read()) {<br><br>        if ($file != "." &amp;&amp; $file != "..") {<br><br>            if (is_dir($src_dir."/".$file)) {<br><br>                if (!@ftp_chdir($conn_id, $dst_dir."/".$file)) {<br><br>                ftp_mkdir($conn_id, $dst_dir."/".$file);<br>                }<br><br>            ftp_copy($src_dir."/".$file, $dst_dir."/".$file);<br>            }<br>            else {<br><br>            $upload = ftp_put($conn_id, $dst_dir."/".$file, $src_dir."/".$file, FTP_BINARY);<br>            }<br>        }<br>    }<br><br>$d-&gt;close();<br>}  

---

Got this cryptic error<br><br>Warning:  ftp_put() [function.ftp-put]: &apos;STOR&apos; not understood in <br>C:\wamp\www\kevtest\ftp_todays.php on line 48<br><br>Found the prob, you can&apos;t put a path to the destination file<br>(even though I can do that in the dos ftp client...?)<br><br>e.g. - this doesn&apos;t work<br>ftp_put($conn, &apos;/www/site/file.html&apos;,&apos;c:/wamp/www/site/file.html&apos;,FTP_BINARY);<br><br>you have to put<br><br>

```
<?php
ftp_chdir($conn, '/www/site/');
ftp_put($conn,'file.html', 'c:/wamp/www/site/file.html', FTP_BINARY );
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.ftp-put.php)

**[To root](/README.md)**