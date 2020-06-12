# fpassthru




<div class="phpcode"><span class="html">
Passthru didn&apos;t work for me for files greater than about 5Mb. Just adding &quot;ob_end_clean()&quot;, all works fine now, including &gt; 50Mb files.<br><br>$ToProtectedFile=$pathUnder.$filename<br>$handle = @fopen($ToProtectedFile, &quot;rb&quot;);<br><br>@header(&quot;Cache-Control: no-cache, must-revalidate&quot;); <br>@header(&quot;Pragma: no-cache&quot;); //keeps ie happy<br>@header(&quot;Content-Disposition: attachment; filename= &quot;.$NomFichier);<br>@header(&quot;Content-type: application/octet-stream&quot;);<br>@header(&quot;Content-Length: &quot;.$SizeOfFile);<br>@header(&apos;Content-Transfer-Encoding: binary&apos;);<br><br>ob_end_clean();//required here or large files will not work<br>@fpassthru($handle);//works fine now</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.fpassthru.php)

**[⬆ to root](/)**