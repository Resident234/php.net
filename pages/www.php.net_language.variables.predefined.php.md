# Predefined Variables




<div class="phpcode"><span class="html">
I haven&apos;t found it anywhere else in the manual, so I&apos;ll make a note of it here - PHP will automatically replace any dots (&apos;.&apos;) in an incoming variable name with underscores (&apos;_&apos;). So if you have dots in your incoming variables, e.g.:<br><br>example.com/page.php?chuck.norris=nevercries<br><br>you can not reference them by the name used in the URI:<br>//INCORRECT<br>echo $_GET[&apos;chuck.norris&apos;];<br><br>instead you must use:<br>//CORRECT<br>echo $_GET[&apos;chuck_norris&apos;];</span>
</div>
  

#


<div class="phpcode"><span class="html">
I have this function in my main files, it allows for easier SEO for some pages without having to rely on .htaccess and mod_rewrite for some things.<br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">long_to_GET</span><span class="keyword">(){<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">/**<br>&#xA0; &#xA0; &#xA0; &#xA0; * This function converts info.php/a/1/b/2/c?d=4 TO<br>&#xA0; &#xA0; &#xA0; &#xA0; * Array ( [d] =&gt; 4 [a] =&gt; 1 [b] =&gt; 2 [c] =&gt; ) <br>&#xA0; &#xA0; &#xA0; &#xA0; **/<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if(isset(</span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;PATH_INFO&apos;</span><span class="keyword">]) &amp;&amp; </span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;PATH_INFO&apos;</span><span class="keyword">] != </span><span class="string">&apos;&apos;</span><span class="keyword">){<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//Split it out.<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$tmp </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos;/&apos;</span><span class="keyword">,</span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;PATH_INFO&apos;</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//Remove first empty item<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">unset(</span><span class="default">$tmp</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//Loop through and apend it into the $_GET superglobal.<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">for(</span><span class="default">$i</span><span class="keyword">=</span><span class="default">1</span><span class="keyword">;</span><span class="default">$i</span><span class="keyword">&lt;=</span><span class="default">count</span><span class="keyword">(</span><span class="default">$tmp</span><span class="keyword">);</span><span class="default">$i</span><span class="keyword">+=</span><span class="default">2</span><span class="keyword">){ </span><span class="default">$_GET</span><span class="keyword">[</span><span class="default">$tmp</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]] = </span><span class="default">$tmp</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">+</span><span class="default">1</span><span class="keyword">];}<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;<br></span><br>Its probably not the most efficient, but it does the job rather nicely.<br><br>DD32</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you&apos;re running PHP as a shell script, and you want to use the argv and argc arrays to get command-line arguments, make sure you have register_argc_argv&#xA0; =&#xA0; on.&#xA0; If you&apos;re using the &apos;optimized&apos; php.ini, this defaults to off.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.variables.predefined.php)

**[To root](/README.md)**