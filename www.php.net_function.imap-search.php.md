# imap_search




<div class="phpcode"><span class="html">
The date format for e.g. SINCE is, according to rfc3501:<br><br>date&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; = date-text / DQUOTE date-text DQUOTE<br><br>date-day&#xA0; &#xA0; &#xA0; &#xA0; = 1*2DIGIT<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ; Day of month<br><br>date-day-fixed&#xA0; = (SP DIGIT) / 2DIGIT<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ; Fixed-format version of date-day<br><br>date-month&#xA0; &#xA0; &#xA0; = &quot;Jan&quot; / &quot;Feb&quot; / &quot;Mar&quot; / &quot;Apr&quot; / &quot;May&quot; / &quot;Jun&quot; /<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;Jul&quot; / &quot;Aug&quot; / &quot;Sep&quot; / &quot;Oct&quot; / &quot;Nov&quot; / &quot;Dec&quot;<br><br>date-text&#xA0; &#xA0; &#xA0;&#xA0; = date-day &quot;-&quot; date-month &quot;-&quot; date-year<br><br>So a valid date is e.g. &quot;22-Jul-2012&quot; with or without the double quotes.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Hi, <br>be aware, that imap_search() does NOT (as you may exspect) return an empty array, if nothing was found! <br>As the manual says, it returns FALSE.<br><br>Do not test the result like &quot;count($array)&quot; as I did. <br>This gives you 1 for an empty result. Took me an hour to found out why :-(&#xA0; RTFM</span>
</div>
  

#


<div class="phpcode"><span class="html">
To set your own CHARSET, which is useful if you are dealing with Chinese Japanese and Korean queries.
<br>
<br><span class="default">&lt;?php imap_search</span><span class="keyword">(</span><span class="default">$inbox</span><span class="keyword">,</span><span class="string">&apos;BODY &quot;&apos;</span><span class="keyword">.</span><span class="default">$keyword</span><span class="keyword">.</span><span class="string">&apos;&quot;&apos;</span><span class="keyword">, </span><span class="default">SE_FREE</span><span class="keyword">, </span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">); </span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-search.php)

**[To root](/README.md)**