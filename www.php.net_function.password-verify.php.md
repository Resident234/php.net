# password_verify




<div class="phpcode"><span class="html">
If you get incorrect false responses from password_verify when manually including the hash variable (eg. for testing) and you know it should be correct, make sure you are enclosing the hash variable in single quotes (&apos;) and not double quotes (&quot;).
<br>
<br>PHP parses anything that starts with a $ inside double quotes as a variable:
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">// this will result in &apos;Invalid Password&apos; as the hash is parsed into 3 variables of
<br>// $2y, $07 and $BCryptRequires22Chrcte/VlQH0piJtjXl.0t1XkA8pw9dMXTpOq
<br>// due to it being enclosed inside double quotes
<br></span><span class="default">$hash </span><span class="keyword">= </span><span class="string">&quot;$2y$07</span><span class="default">$BCryptRequires22Chrcte</span><span class="string">/VlQH0piJtjXl.0t1XkA8pw9dMXTpOq&quot;</span><span class="keyword">;
<br>
<br></span><span class="comment">// this will result in &apos;Password is valid&apos; as variables are not parsed inside single quotes
<br></span><span class="default">$hash </span><span class="keyword">= </span><span class="string">&apos;$2y$07$BCryptRequires22Chrcte/VlQH0piJtjXl.0t1XkA8pw9dMXTpOq&apos;</span><span class="keyword">;
<br>
<br>if (</span><span class="default">password_verify</span><span class="keyword">(</span><span class="string">&apos;rasmuslerdorf&apos;</span><span class="keyword">, </span><span class="default">$hash</span><span class="keyword">)) {
<br>&#xA0; &#xA0; echo </span><span class="string">&apos;Password is valid!&apos;</span><span class="keyword">;
<br>} else {
<br>&#xA0; &#xA0; echo </span><span class="string">&apos;Invalid password.&apos;</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
This function can be used to verify hashes created with other functions like crypt(). For example:<br><br><span class="default">&lt;?php<br><br>$hash </span><span class="keyword">= </span><span class="string">&apos;$1$toHVx1uW$KIvW9yGZZSU/1YOidHeqJ/&apos;</span><span class="keyword">;<br><br>if (</span><span class="default">password_verify</span><span class="keyword">(</span><span class="string">&apos;rasmuslerdorf&apos;</span><span class="keyword">, </span><span class="default">$hash</span><span class="keyword">)) {<br>&#xA0; &#xA0; echo </span><span class="string">&apos;Password is valid!&apos;</span><span class="keyword">;<br>} else {<br>&#xA0; &#xA0; echo </span><span class="string">&apos;Invalid password.&apos;</span><span class="keyword">;<br>}<br><br></span><span class="comment">// Output: Password is valid!<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
The function password_verify() uses constant time. This makes it safe against timing attacks. Don&apos;t use crypt($password_database) === crypt($password_given_by_login), since there is no protection against timing attacks!<br><br>If you don&apos;t want to use password_verify(), then have a look at hash_equals(), which also runs a timing attack safe string comparison.</span>
</div>
  

#


<div class="phpcode"><span class="html">
This Is The Most Secure Way To Keep Your Password Safe With PHP 7 , <br>Even When Your DataBase Has Been Hacked ,<br>It Will Be Almost Impossible To Retrieve Your Password .<br>--------------------------------------------------------<br>--- When A User Wants To Sign Up ---<br>1 ---&gt; Get Input From User Which Is The User`s Password<br>1 ---&gt; Hash The Password<br>2 ---&gt; Store The Hashed Password In Your DataBase<br>--------------------------------------------------------<br><span class="default">&lt;?php<br>$hashed_password </span><span class="keyword">= </span><span class="default">password_hash</span><span class="keyword">(</span><span class="default">$_POST</span><span class="keyword">[</span><span class="string">&quot;password&quot;</span><span class="keyword">],</span><span class="default">PASSWORD_DEFAULT</span><span class="keyword">);<br><br></span><span class="comment">// $_POST[&quot;password&quot;] ---&gt; Is The User`s Input<br>// $hashed_password ---&gt; Is The Hashed Password You Can Store In Your DataBase<br></span><span class="default">?&gt;<br></span>--------------------------------------------------------<br>--- When A User Wants To Sign In ---<br>1 ---&gt; Get Input From User Which Is The User`s Password<br>2 ---&gt; Fetch The Hashed Password From Your Database<br>3 ---&gt; Compare The User`s Input And The Hashed Password <br>--------------------------------------------------------<br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">if(</span><span class="default">password_verify</span><span class="keyword">(</span><span class="default">$_POST</span><span class="keyword">[</span><span class="string">&quot;password&quot;</span><span class="keyword">],</span><span class="default">$hashed_password</span><span class="keyword">))<br>&#xA0; &#xA0; echo </span><span class="string">&quot;Welcome&quot;</span><span class="keyword">; <br><br>&#xA0; &#xA0; else<br>&#xA0; &#xA0; echo </span><span class="string">&quot;Wrong Password&quot;</span><span class="keyword">;<br><br></span><span class="comment">// $_POST[&quot;password&quot;] ---&gt; Is The User`s Input<br>// $hashed_password ---&gt; Is The Hashed Password You Have Fetched From DataBase<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.password-verify.php)

**[To root](/README.md)**