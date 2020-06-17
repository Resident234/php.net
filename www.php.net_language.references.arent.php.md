# What References Are Not




<div class="phpcode"><span class="html">
What References are not: References.<br><br>References are opaque things that are like pointers, except A) smarter and B) referring to HLL objects, rather than memory addresses. PHP doesn&apos;t have references. PHP has a syntax for creating *aliases* which are multiple names for the same object. PHP has a few pieces of syntax for calling and returning &quot;by reference&quot;, which really just means inhibiting copying. At no point in this &quot;references&quot; section of the manual are there any references.</span>
</div>
  

#


<div class="phpcode"><span class="html">
The example given in the text:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">foo</span><span class="keyword">(&amp;</span><span class="default">$var</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; </span><span class="default">$var </span><span class="keyword">=&amp; </span><span class="default">$GLOBALS</span><span class="keyword">[</span><span class="string">&quot;baz&quot;</span><span class="keyword">];<br>}<br></span><span class="default">foo</span><span class="keyword">(</span><span class="default">$bar</span><span class="keyword">); <br></span><span class="default">?&gt;<br></span><br>illustrates (to my mind anyway) why the = and &amp; should be written together as a new kind of replacement operator and not apart as in C, like&#xA0; $var = &amp;$GLOBALS[&quot;baz&quot;];<br><br>Using totally new terminology:<br><br>To me the result of this function is not surprising because the =&amp; means &apos;change the &quot;destination&quot; of $var from wherever it was to the same as the destination of $GLOBALS[&quot;baz&quot;]. Well it &apos;was&apos; the actual parameter $bar, but now it will be the global at &quot;baz&quot;.<br><br>If you simply remove the &amp; in the the replacement, it will place the value of $GLOBALS[&quot;baz&apos;] into the destination of $var, which is $bar (unless $bar was already a reference, then the value goes into that destination.)<br><br>To summarize, =, replaces the &apos;destination&apos;s value; =&amp;, changes the destination.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.references.arent.php)

**[To root](/README.md)**