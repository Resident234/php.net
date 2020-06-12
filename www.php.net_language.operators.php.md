# Operators




<div class="phpcode"><span class="html">
of course this should be clear, but i think it has to be mentioned espacially:<br><br>AND is not the same like &amp;&amp;<br><br>for example:<br><br><span class="default">&lt;?php $a </span><span class="keyword">&amp;&amp; </span><span class="default">$b </span><span class="keyword">|| </span><span class="default">$c</span><span class="keyword">; </span><span class="default">?&gt;<br></span>is not the same like<br><span class="default">&lt;?php $a </span><span class="keyword">AND </span><span class="default">$b </span><span class="keyword">|| </span><span class="default">$c</span><span class="keyword">; </span><span class="default">?&gt;<br></span><br>the first thing is<br>(a and b) or c<br><br>the second<br>a and (b or c)<br><br>&apos;cause || has got a higher priority than and, but less than &amp;&amp;<br><br>of course, using always [ &amp;&amp; and || ] or [ AND and OR ] would be okay, but than you should at least respect the following:<br><br><span class="default">&lt;?php $a </span><span class="keyword">= </span><span class="default">$b </span><span class="keyword">&amp;&amp; </span><span class="default">$c</span><span class="keyword">; </span><span class="default">?&gt;<br>&lt;?php $a </span><span class="keyword">= </span><span class="default">$b </span><span class="keyword">AND </span><span class="default">$c</span><span class="keyword">; </span><span class="default">?&gt;<br></span><br>the first code will set $a to the result of the comparison $b with $c, both have to be true, while the second code line will set $a like $b and THAN - after that - compare the success of this with the value of $c<br><br>maybe usefull for some tricky coding and helpfull to prevent bugs :D<br><br>greetz, Warhog</span>
</div>
  

#


<div class="phpcode"><span class="html">
Other Language books&apos; operator precedence section usually include &quot;(&quot; and &quot;)&quot; - with exception of a Perl book that I have. (In PHP &quot;{&quot; and &quot;}&quot; should also be considered also). However, PHP Manual is not listed &quot;(&quot; and &quot;)&quot; in precedence list. It looks like &quot;(&quot; and &quot;)&quot; has higher precedence as it should be.
<br>
<br>Note: If you write following code, you would need &quot;()&quot; to get expected value.
<br>
<br><span class="default">&lt;?php
<br>$bar </span><span class="keyword">= </span><span class="default">true</span><span class="keyword">;
<br></span><span class="default">$str </span><span class="keyword">= </span><span class="string">&quot;TEST&quot;</span><span class="keyword">. (</span><span class="default">$bar </span><span class="keyword">? </span><span class="string">&apos;true&apos; </span><span class="keyword">: </span><span class="string">&apos;false&apos;</span><span class="keyword">) .</span><span class="string">&quot;TEST&quot;</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>Without &quot;(&quot; and &quot;)&quot; you will get only &quot;true&quot; in $str. 
<br>(PHP4.0.4pl1/Apache DSO/Linux, PHP4.0.5RC1/Apache DSO/W2K Server)
<br>It&apos;s due to precedence, probably.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.php)

**[To root](/)**