# The InvalidArgumentException class




<div class="phpcode"><span class="html">
In my opinion this exception is invaluable for validating arguments- for example providing strict typing a la C:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">tripleInteger</span><span class="keyword">(</span><span class="default">$int</span><span class="keyword">)<br>{<br>&#xA0; if(!</span><span class="default">is_int</span><span class="keyword">(</span><span class="default">$int</span><span class="keyword">))<br>&#xA0; &#xA0; throw new </span><span class="default">InvalidArgumentException</span><span class="keyword">(</span><span class="string">&apos;tripleInteger function only accepts integers. Input was: &apos;</span><span class="keyword">.</span><span class="default">$int</span><span class="keyword">);<br>&#xA0; return </span><span class="default">$int </span><span class="keyword">* </span><span class="default">3</span><span class="keyword">;<br>}<br><br></span><span class="default">$x </span><span class="keyword">= </span><span class="default">tripleInteger</span><span class="keyword">(</span><span class="default">4</span><span class="keyword">); </span><span class="comment">//$x == 12<br></span><span class="default">$x </span><span class="keyword">= </span><span class="default">tripleInteger</span><span class="keyword">(</span><span class="default">2.5</span><span class="keyword">); </span><span class="comment">//exception will be thrown as 2.5 is a float<br></span><span class="default">$x </span><span class="keyword">= </span><span class="default">tripleInteger</span><span class="keyword">(</span><span class="string">&apos;foo&apos;</span><span class="keyword">); </span><span class="comment">//exception will be thrown as &apos;foo&apos; is a string<br></span><span class="default">$x </span><span class="keyword">= </span><span class="default">tripleInteger</span><span class="keyword">(</span><span class="string">&apos;4&apos;</span><span class="keyword">); </span><span class="comment">//exception will throw as &apos;4&apos; is also a string<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.invalidargumentexception.php)

**[To root](/README.md)**