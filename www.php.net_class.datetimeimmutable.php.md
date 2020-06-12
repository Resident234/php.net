# The DateTimeImmutable class




<div class="phpcode"><span class="html">
Note: If you are trying to get 02.29 for non leap year it will return 03.01<br>Example: <br><span class="default">&lt;?php <br></span><span class="keyword">new </span><span class="default">DateTimeImmutable</span><span class="keyword">(</span><span class="string">&apos;2017-02-29&apos;</span><span class="keyword">) </span><span class="comment">// 2017-03-01<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here&apos;s a simple example on how this class works :<br><br>&#xA0; &#xA0; // Create a DateTimeImmutable Object<br>&#xA0; &#xA0; $date = new DateTimeImmutable(&apos;2000-01-01&apos;);<br>&#xA0; &#xA0; // &quot;Change&quot; that object and assign it&apos;s value to a new variable<br>&#xA0; &#xA0; $date2 = $date-&gt;add(new DateInterval(&apos;P6M5DT24H&apos;));<br>&#xA0; &#xA0; // Check out the content of the two variables<br>&#xA0; &#xA0; echo $date-&gt;format(&apos;Y-m-d&apos;) . &quot;\n&quot;;<br>&#xA0; &#xA0; // 2000-01-01<br>&#xA0; &#xA0; echo $date2-&gt;format(&apos;Y-m-d&apos;) . &quot;\n&quot;;<br>&#xA0; &#xA0; // 2000-07-07</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.datetimeimmutable.php)

**[To root](/README.md)**