# compact




<div class="phpcode"><span class="html">
Can also handy for debugging, to quickly show a bunch of variables and their values:<br><br><span class="default">&lt;?php<br>print_r</span><span class="keyword">(</span><span class="default">compact</span><span class="keyword">(</span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos; &apos;</span><span class="keyword">, </span><span class="string">&apos;count acw cols coldepth&apos;</span><span class="keyword">)));<br></span><span class="default">?&gt;<br></span><br>gives<br><br>Array<br>(<br>&#xA0; &#xA0; [count] =&gt; 70<br>&#xA0; &#xA0; [acw] =&gt; 9<br>&#xA0; &#xA0; [cols] =&gt; 7<br>&#xA0; &#xA0; [coldepth] =&gt; 10<br>)</span>
</div>
  

#


<div class="phpcode"><span class="html">
So compact(&apos;var1&apos;, &apos;var2&apos;) is the same as saying array(&apos;var1&apos; =&gt; $var1, &apos;var2&apos; =&gt; $var2) as long as $var1 and $var2 are set.</span>
</div>
  

#


<div class="phpcode"><span class="html">
The description says that compact is the opposite of extract() but it is important to understand that it does not completely reverse extract().&#xA0; In particluar compact() does not unset() the argument variables given to it (and that extract() may have created).&#xA0; If you want the individual variables to be unset after they are combined into an array then you have to do that yourself.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.compact.php)

**[To root](/README.md)**