# fpassthru





Passthru didn&apos;t work for me for files greater than about 5Mb. Just adding &quot;ob_end_clean()&quot;, all works fine now, including &gt; 50Mb files.

$ToProtectedFile=$pathUnder.$filename
$handle = @fopen($ToProtectedFile, &quot;rb&quot;);

@header(&quot;Cache-Control: no-cache, must-revalidate&quot;); 
@header(&quot;Pragma: no-cache&quot;); //keeps ie happy
@header(&quot;Content-Disposition: attachment; filename= &quot;.$NomFichier);
@header(&quot;Content-type: application/octet-stream&quot;);
@header(&quot;Content-Length: &quot;.$SizeOfFile);
@header(&apos;Content-Transfer-Encoding: binary&apos;);

ob_end_clean();//required here or large files will not work
@fpassthru($handle);//works fine now

  

#

[Official documentation page](https://www.php.net/manual/en/function.fpassthru.php)

**[To root](/README.md)**