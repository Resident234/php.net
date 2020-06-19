# gnupg_encrypt




<div class="phpcode"><span class="html">
After spending some time trying to get this extension to work, I&apos;ve found that you have to have the GNUPGHOME environment variable set so that the keychain can be found, and have it set equal to the .gnupg directory itself, not the apache/httpd user&apos;s home directory (which is what is shown in dan&apos;s example code).&#xA0; below is an example of this and a simple function I was working on at the time to encrypt a piece of data for storage in a database.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="comment">// set the environment so gnupg can find the keyring<br>&#xA0; &#xA0; </span><span class="default">putenv</span><span class="keyword">(</span><span class="string">&quot;GNUPGHOME=/home/apache/.gnupg&quot;</span><span class="keyword">);<br><br>&#xA0; &#xA0; function </span><span class="default">encrypt_string</span><span class="keyword">(</span><span class="default">$str</span><span class="keyword">,</span><span class="default">$fingerprint</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$res </span><span class="keyword">= </span><span class="default">gnupg_init</span><span class="keyword">();<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">gnupg_addencryptkey</span><span class="keyword">(</span><span class="default">$res</span><span class="keyword">,</span><span class="default">$fingerprint</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$enc </span><span class="keyword">= </span><span class="default">gnupg_encrypt</span><span class="keyword">(</span><span class="default">$res</span><span class="keyword">, </span><span class="default">$str</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$enc</span><span class="keyword">;<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.gnupg-encrypt.php)

**[To root](/README.md)**