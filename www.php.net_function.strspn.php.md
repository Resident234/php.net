# strspn




<div class="phpcode"><span class="html">
you can use this function with strlen to check illegal characters, string lenght must be the same than strspn (characters from my string contained in another)<br><br><span class="default">&lt;?php<br><br>$digits</span><span class="keyword">=</span><span class="string">&apos;0123456789&apos;</span><span class="keyword">;<br><br>if (</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$phone</span><span class="keyword">) != </span><span class="default">strspn</span><span class="keyword">(</span><span class="default">$phone</span><span class="keyword">,</span><span class="default">$digits</span><span class="keyword">))<br> echo </span><span class="string">&quot;illegal characters&quot;</span><span class="keyword">;<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
It took me some time to understand the way this function works&#x2026;<br>I&#x2019;ve compiled my own explanation with my own words that is more understandable for me personally than the official one or those that can be found in different tutorials on the web.<br>Perhaps, it will save someone several minutes&#x2026;<br><br><span class="default">&lt;?php <br>strspn</span><span class="keyword">(</span><span class="default">string $haystack</span><span class="keyword">, </span><span class="default">string $char_list </span><span class="keyword">[, </span><span class="default">int $start </span><span class="keyword">[, </span><span class="default">int $length</span><span class="keyword">]])<br></span><span class="default">?&gt;<br></span><br>The way it works:<br> -&#xA0;&#xA0; searches for a segment of $haystack that consists entirely from supplied through the second argument chars <br> -&#xA0;&#xA0; $haystack must start from one of the chars supplied through $char_list, otherwise the function will find nothing<br> -&#xA0;&#xA0; as soon as the function encounters a char that was not mentioned in $chars it understands that the segment is over and stops (it doesn&#x2019;t search for the second, third and so on segments)<br> -&#xA0;&#xA0; finally, it measures the segment&#x2019;s length and return it (i.e. length)<br><br>In other words it finds a span (only the first one) in the string that consists entirely form chars supplied in $chars_list and returns its length</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.strspn.php)

**[⬆ to root](/)**