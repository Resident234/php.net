# get_defined_vars




<div class="phpcode"><span class="html">
dirty code sample:<br><br>print_r(compact(array_keys(get_defined_vars())));</span>
</div>
  

#


<div class="phpcode"><span class="html">
A little gotcha to watch out for:<br><br>If you turn off RegisterGlobals and related, then use get_defined_vars(), you may see something like the following:<br><br><span class="default">&lt;?php<br></span><span class="keyword">Array<br>(<br>&#xA0; &#xA0; [</span><span class="default">GLOBALS</span><span class="keyword">] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [</span><span class="default">GLOBALS</span><span class="keyword">] =&gt; Array<br> *</span><span class="default">RECURSION</span><span class="keyword">*<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [</span><span class="default">_POST</span><span class="keyword">] =&gt; Array()<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [</span><span class="default">_GET</span><span class="keyword">] =&gt; Array()<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [</span><span class="default">_COOKIE</span><span class="keyword">] =&gt; Array()<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [</span><span class="default">_FILES</span><span class="keyword">] =&gt; Array()<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; [</span><span class="default">_POST</span><span class="keyword">] =&gt; Array()<br>&#xA0; &#xA0; [</span><span class="default">_GET</span><span class="keyword">] =&gt; Array()<br>&#xA0; &#xA0; [</span><span class="default">_COOKIE</span><span class="keyword">] =&gt; Array()<br>&#xA0; &#xA0; [</span><span class="default">_FILES</span><span class="keyword">] =&gt; Array()<br><br>)<br></span><span class="default">?&gt;<br></span><br>Notice that $_SERVER isn&apos;t there.&#xA0; It seems that php only loads the superglobal $_SERVER if it is used somewhere.&#xA0; You could do this:<br><br><span class="default">&lt;?php<br></span><span class="keyword">print </span><span class="string">&apos;&lt;pre&gt;&apos; </span><span class="keyword">. </span><span class="default">htmlspecialchars</span><span class="keyword">(</span><span class="default">print_r</span><span class="keyword">(</span><span class="default">get_defined_vars</span><span class="keyword">(), </span><span class="default">true</span><span class="keyword">)) . </span><span class="string">&apos;&lt;/pre&gt;&apos;</span><span class="keyword">;<br>print </span><span class="string">&apos;&lt;pre&gt;&apos; </span><span class="keyword">. </span><span class="default">htmlspecialchars</span><span class="keyword">(</span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$_SERVER</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">)) . </span><span class="string">&apos;&lt;/pre&gt;&apos;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>And then $_SERVER will appear in both lists.&#xA0; I guess it&apos;s not really a gotcha, because nothing bad will happen either way, but it&apos;s an interesting curiosity nonetheless.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.get-defined-vars.php)

**[To root](/README.md)**