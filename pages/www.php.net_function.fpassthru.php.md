# fpassthru



Passthru didn&apos;t work for me for files greater than about 5Mb. Just adding "ob_end_clean()", all works fine now, including &gt; 50Mb files.<br><br>$ToProtectedFile=$pathUnder.$filename<br>$handle = @fopen($ToProtectedFile, "rb");<br><br>@header("Cache-Control: no-cache, must-revalidate"); <br>@header("Pragma: no-cache"); //keeps ie happy<br>@header("Content-Disposition: attachment; filename= ".$NomFichier);<br>@header("Content-type: application/octet-stream");<br>@header("Content-Length: ".$SizeOfFile);<br>@header(&apos;Content-Transfer-Encoding: binary&apos;);<br><br>ob_end_clean();//required here or large files will not work<br>@fpassthru($handle);//works fine now  

---

[Official documentation page](https://www.php.net/manual/en/function.fpassthru.php)

**[To root](/README.md)**