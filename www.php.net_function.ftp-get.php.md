# ftp_get




<div class="phpcode"><span class="html">
I tried to ftp a 7mb file today off my webserver.<br><br>I copied this example directly and it told me.<br><br>Port command successful<br>&quot;there was a problem&quot;<br><br>I thought it was because of the size.<br>But I guessed it might be cause of my firewall.<br><br>So I made the ftp connection passive:<br><br><span class="default">&lt;?PHP<br>&#xA0; <br>&#xA0; </span><span class="keyword">...<br>&#xA0; </span><span class="default">$login_result </span><span class="keyword">= </span><span class="default">ftp_login</span><span class="keyword">(</span><span class="default">$conn_id</span><span class="keyword">, </span><span class="default">$ftp_user_name</span><span class="keyword">, </span><span class="default">$ftp_user_pass</span><span class="keyword">);<br>&#xA0; </span><span class="default">ftp_pasv</span><span class="keyword">(</span><span class="default">$conn_id</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>Ran the script again &amp; it worked fine.<br><br>Hope this helps someone</span>
</div>
  

#


<div class="phpcode"><span class="html">
Don&apos;t want to use an intermediate file?&#xA0; Use &apos;php://output&apos; as the filename and then capture the output using output buffering.<br><br>ob_start();<br>$result = ftp_get($ftp, &quot;php://output&quot;, $file, FTP_BINARY);<br>$data = ob_get_contents();<br>ob_end_clean();<br><br>Don&apos;t forget to check $result to make sure there wasn&apos;t an error.&#xA0; After that, manipulate the $data variable however you want.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Keep in mind that ftp_get will overwrite the file on your local machine if it has the same name.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ftp-get.php)

**[⬆ to root](/)**