# Transliterator::transliterate




<div class="phpcode"><span class="html">
I pretty much like the idea of hdogan, but there&apos;s at least one group of characters he&apos;s missing: ligature characters.<br>They&apos;re at least used in Norwegian and I read something about French, too ... Some are just used for styling (f.e. &#xFB01;)<br><br>Here&apos;s an example that supports all characters (should at least, according to the documentation):<br><span class="default">&lt;?php<br>var_dump</span><span class="keyword">(</span><span class="default">transliterator_transliterate</span><span class="keyword">(</span><span class="string">&apos;Any-Latin; Latin-ASCII; Lower()&apos;</span><span class="keyword">, </span><span class="string">&quot;A &#xE6; &#xDC;b&#xE9;rmensch p&#xE5; h&#xF8;yeste niv&#xE5;! &#x418; &#x44F; &#x43B;&#x44E;&#x431;&#x43B;&#x44E; PHP! &#xFB01;&quot;</span><span class="keyword">));<br></span><span class="comment">// string(41) &quot;a ae ubermensch pa hoyeste niva! i a lublu php! fi&quot;<br></span><span class="default">?&gt;<br></span><br>In this example any character will firstly be converted to a latin character. If that&apos;s finished, replace all latin characters by their ASCII replacement.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/transliterator.transliterate.php)

**[To root](/README.md)**