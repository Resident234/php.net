# stristr




<div class="phpcode"><span class="html">
There was a change in PHP 4.2.3 that can cause a warning message<br>to be generated when using stristr(), even though no message was<br>generated in older versions of PHP.<br><br>The following will generate a warning message in 4.0.6 and 4.2.3:<br>&#xA0; stristr(&quot;haystack&quot;, &quot;&quot;);<br>&#xA0; &#xA0;&#xA0; OR<br>&#xA0; $needle = &quot;&quot;;&#xA0; stristr(&quot;haystack&quot;, $needle);<br><br>This will _not_ generate an &quot;Empty Delimiter&quot; warning message in<br>4.0.6, but _will_ in 4.2.3:<br>&#xA0; unset($needle); stristr(&quot;haystack&quot;, $needle);<br><br>Here&apos;s a URL that documents what was changed:<br><a href="http://groups.google.ca/groups?selm=cvshholzgra1031224321%40cvsserver" rel="nofollow" target="_blank">http://groups.google.ca/groups?selm=cvshholzgra1031224321%40cvsserver</a></span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.stristr.php)

**[⬆ to root](/)**