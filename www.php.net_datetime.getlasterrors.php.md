# DateTime::getLastErrors




<div class="phpcode"><span class="html">
DateTime::createFromFormat is smart to handle the cases where you input an invalid date, like April 31st, and convert it to May 1st. In some cases, you do not want this automatic smart handling of the dates for example in a user input form where you want to be sure that your user did input the date he wanted. To do that, you need to get access to the warnings, this method is the only way to do it:<br><br><span class="default">&lt;?php<br>$date </span><span class="keyword">= </span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">createFromFormat</span><span class="keyword">(</span><span class="string">&apos;Y-m-d&apos;</span><span class="keyword">, </span><span class="string">&apos;1999-04-31&apos;</span><span class="keyword">);<br>print </span><span class="default">$date</span><span class="keyword">-&gt;</span><span class="default">format</span><span class="keyword">(</span><span class="string">&apos;Y-m-d&apos;</span><span class="keyword">) . </span><span class="default">PHP_EOL</span><span class="keyword">;<br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">DateTime</span><span class="keyword">::</span><span class="default">getLastErrors</span><span class="keyword">());<br></span><span class="default">?&gt;<br></span><br>The output is:<br><br>1999-05-01<br>Array<br>(<br>&#xA0; &#xA0; [warning_count] =&gt; 1<br>&#xA0; &#xA0; [warnings] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [10] =&gt; The parsed date was invalid<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; [error_count] =&gt; 0<br>&#xA0; &#xA0; [errors] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>)<br><br>So, here you can see, you have a warning because the date was invalid, but not an error because PHP was smart enough to convert it into a valid date. It is then up to you to do something with this information.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/datetime.getlasterrors.php)

**[To root](/README.md)**