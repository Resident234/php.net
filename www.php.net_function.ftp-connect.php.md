# ftp_connect




<div class="phpcode"><span class="html">
Ever needed to create an FTP connection resource defaulted to a particular dir from a URI? Here&apos;s a simple function that will take a URI like <a href="ftp://username:password@subdomain.example.com/path1/path2/," rel="nofollow" target="_blank">ftp://username:password@subdomain.example.com/path1/path2/,</a> and return an FTP connection resource.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">getFtpConnection</span><span class="keyword">(</span><span class="default">$uri</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; </span><span class="comment">// Split FTP URI into:
<br>&#xA0; &#xA0; // $match[0] = <a href="ftp://username:password@sld.domain.tld/path1/path2/" rel="nofollow" target="_blank">ftp://username:password@sld.domain.tld/path1/path2/</a>
<br>&#xA0; &#xA0; // $match[1] = <a href="ftp://" rel="nofollow" target="_blank">ftp://</a>
<br>&#xA0; &#xA0; // $match[2] = username
<br>&#xA0; &#xA0; // $match[3] = password
<br>&#xA0; &#xA0; // $match[4] = sld.domain.tld
<br>&#xA0; &#xA0; // $match[5] = /path1/path2/
<br>&#xA0; &#xA0; </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&quot;/ftp:\/\/(.*?):(.*?)@(.*?)(\/.*)/i&quot;</span><span class="keyword">, </span><span class="default">$uri</span><span class="keyword">, </span><span class="default">$match</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; </span><span class="comment">// Set up a connection
<br>&#xA0; &#xA0; </span><span class="default">$conn </span><span class="keyword">= </span><span class="default">ftp_connect</span><span class="keyword">(</span><span class="default">$match</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] . </span><span class="default">$match</span><span class="keyword">[</span><span class="default">4</span><span class="keyword">] . </span><span class="default">$match</span><span class="keyword">[</span><span class="default">5</span><span class="keyword">]);
<br>
<br>&#xA0; &#xA0; </span><span class="comment">// Login
<br>&#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">ftp_login</span><span class="keyword">(</span><span class="default">$conn</span><span class="keyword">, </span><span class="default">$match</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">], </span><span class="default">$match</span><span class="keyword">[</span><span class="default">3</span><span class="keyword">]))
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Change the dir
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">ftp_chdir</span><span class="keyword">(</span><span class="default">$conn</span><span class="keyword">, </span><span class="default">$match</span><span class="keyword">[</span><span class="default">5</span><span class="keyword">]);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Return the resource
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">$conn</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; </span><span class="comment">// Or retun null
<br>&#xA0; &#xA0; </span><span class="keyword">return </span><span class="default">null</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.ftp-connect.php)

**[To root](/README.md)**