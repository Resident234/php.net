# password_get_info




<div class="phpcode"><span class="html">
If you&apos;re curious to use this method to determine if there is someway to evaluate if a given string is NOT a password_hash() value...<br><br><span class="default">&lt;?php<br><br></span><span class="comment">// Our password.. the kind of thing and idiot would have on his luggage:<br></span><span class="default">$password_plaintext </span><span class="keyword">= </span><span class="string">&quot;12345&quot;</span><span class="keyword">;<br><br></span><span class="comment">// Hash it up, fuzzball!<br></span><span class="default">$password_hash </span><span class="keyword">= </span><span class="default">password_hash</span><span class="keyword">( </span><span class="default">$password_plaintext</span><span class="keyword">, </span><span class="default">PASSWORD_DEFAULT</span><span class="keyword">, [ </span><span class="string">&apos;cost&apos; </span><span class="keyword">=&gt; </span><span class="default">11 </span><span class="keyword">] );<br><br></span><span class="comment">// What do we get?<br></span><span class="default">print_r</span><span class="keyword">( </span><span class="default">password_get_info</span><span class="keyword">( </span><span class="default">$password_hash </span><span class="keyword">) );<br><br></span><span class="comment">/* returns:<br>Array ( <br>&#xA0; &#xA0; [algo] =&gt; 1 <br>&#xA0; &#xA0; [algoName] =&gt; bcrypt&#xA0; // Your server&apos;s default.<br>&#xA0; &#xA0; [options] =&gt; Array ( [cost] =&gt; 11 ) <br>)<br>*/<br><br>// What about if it&apos;s un-hashed?...<br></span><span class="default">print_r</span><span class="keyword">( </span><span class="default">password_get_info</span><span class="keyword">( </span><span class="default">$password_plaintext </span><span class="keyword">) );<br><br></span><span class="comment">/* returns:<br>Array ( <br>&#xA0; &#xA0; [algo] =&gt; 0 <br>&#xA0; &#xA0; [algoName] =&gt; unknown <br>&#xA0; &#xA0; [options] =&gt; Array ( ) <br>) <br>*/<br></span><span class="default">?&gt;<br></span><br>... Looks like it&apos;s up to each of us to personally decide if it&apos;s safe to compare against the final returned array.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.password-get-info.php)

**[To root](/README.md)**