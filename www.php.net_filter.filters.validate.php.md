# Validate filters




<div class="phpcode"><span class="html">
FILTER_VALIDATE_URL does not work with URNs, examples of valid URIs according to RFC3986 and if they are accepted by FILTER_VALIDATE_URL:
<br>
<br>[PASS] <a href="ftp://ftp.is.co.za.example.org/rfc/rfc1808.txt" rel="nofollow" target="_blank">ftp://ftp.is.co.za.example.org/rfc/rfc1808.txt</a>
<br>[PASS] gopher://spinaltap.micro.umn.example.edu/00/Weather/California/Los%20Angeles
<br>[PASS] <a href="http://www.math.uio.no.example.net/faq/compression-faq/part1.html" rel="nofollow" target="_blank">http://www.math.uio.no.example.net/faq/compression-faq/part1.html</a>
<br>[PASS] <a href="mailto:mduerst@ifi.unizh.example.gov" rel="nofollow" target="_blank">mailto:mduerst@ifi.unizh.example.gov</a>
<br>[PASS] news:comp.infosystems.www.servers.unix
<br>[PASS] telnet://melvyl.ucop.example.edu/
<br>[PASS] <a href="http://www.ietf.org/rfc/rfc2396.txt" rel="nofollow" target="_blank">http://www.ietf.org/rfc/rfc2396.txt</a>
<br>[PASS] ldap://[2001:db8::7]/c=GB?objectClass?one
<br>[PASS] <a href="mailto:John.Doe@example.com" rel="nofollow" target="_blank">mailto:John.Doe@example.com</a>
<br>[PASS] news:comp.infosystems.www.servers.unix
<br>[FAIL] tel:+1-816-555-1212
<br>[PASS] telnet://192.0.2.16:80/
<br>[FAIL] urn:oasis:names:specification:docbook:dtd:xml:4.1.2</span>
</div>
  

#


<div class="phpcode"><span class="html">
Notably missing is a way to validate text entry as printable,<br>printable multiline,<br>or printable and safe (tag free)<br><br>FILTER_VALIDATE_TEXT, which validates no special characters<br>perhaps with FILTER_FLAG_ALLOW_NEWLINE<br>and FILTER_FLAG_NOTAG to disallow tag starters</span>
</div>
  

#


<div class="phpcode"><span class="html">
FILTER_VALIDATE_EMAIL does NOT allow incomplete e-mail addresses to be validated as mentioned by Tomas.
<br>
<br>Using the following code:
<br>
<br><span class="default">&lt;?php
<br>$email </span><span class="keyword">= </span><span class="string">&quot;clifton@example&quot;</span><span class="keyword">; </span><span class="comment">//Note the .com missing
<br></span><span class="keyword">echo </span><span class="string">&quot;PHP Version: &quot;</span><span class="keyword">.</span><span class="default">phpversion</span><span class="keyword">().</span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">;
<br>if(</span><span class="default">filter_var</span><span class="keyword">(</span><span class="default">$email</span><span class="keyword">, </span><span class="default">FILTER_VALIDATE_EMAIL</span><span class="keyword">)){
<br>&#xA0; &#xA0; echo </span><span class="default">$email</span><span class="keyword">.</span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">filter_var</span><span class="keyword">(</span><span class="default">$email</span><span class="keyword">, </span><span class="default">FILTER_VALIDATE_EMAIL</span><span class="keyword">));
<br>}else{
<br>&#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">filter_var</span><span class="keyword">(</span><span class="default">$email</span><span class="keyword">, </span><span class="default">FILTER_VALIDATE_EMAIL</span><span class="keyword">));&#xA0; &#xA0; 
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Returns:
<br>PHP Version: 5.2.14 //On MY server, may be different depending on which version you have installed.
<br>bool(false)
<br>
<br>While the following code:
<br>
<br><span class="default">&lt;?php
<br>$email </span><span class="keyword">= </span><span class="string">&quot;clifton@example.com&quot;</span><span class="keyword">; </span><span class="comment">//Note the .com added
<br></span><span class="keyword">echo </span><span class="string">&quot;PHP Version: &quot;</span><span class="keyword">.</span><span class="default">phpversion</span><span class="keyword">().</span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">;
<br>if(</span><span class="default">filter_var</span><span class="keyword">(</span><span class="default">$email</span><span class="keyword">, </span><span class="default">FILTER_VALIDATE_EMAIL</span><span class="keyword">)){
<br>&#xA0; &#xA0; echo </span><span class="default">$email</span><span class="keyword">.</span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">filter_var</span><span class="keyword">(</span><span class="default">$email</span><span class="keyword">, </span><span class="default">FILTER_VALIDATE_EMAIL</span><span class="keyword">));
<br>}else{
<br>&#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">filter_var</span><span class="keyword">(</span><span class="default">$email</span><span class="keyword">, </span><span class="default">FILTER_VALIDATE_EMAIL</span><span class="keyword">));&#xA0; &#xA0; 
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Returns:
<br>PHP Version: 5.2.14 //On MY server, may be different depending on which version you have installed.
<br>clifton@example.com
<br>string(16) &quot;clifton@example.com&quot;
<br>
<br>This feature is only available for PHP Versions (PHP 5 &gt;= 5.2.0) according to documentation. So make sure your version is correct.
<br>
<br>Cheers,
<br>Clifton</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/filter.filters.validate.php)

**[To root](/README.md)**