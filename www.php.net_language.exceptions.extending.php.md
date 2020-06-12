# Extending Exceptions




<div class="phpcode"><span class="html">
As previously noted exception linking was recently added (and what a god-send it is, it certainly makes layer abstraction (and, by association, exception tracking) easier).<br><br>Since &lt;5.3 was lacking this useful feature I took some initiative and creating a custom exception class that all of my exceptions inherit from:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">SystemException </span><span class="keyword">extends </span><span class="default">Exception<br></span><span class="keyword">{<br>&#xA0; &#xA0; private </span><span class="default">$previous</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; public function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$message</span><span class="keyword">, </span><span class="default">$code </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">, </span><span class="default">Exception $previous </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">parent</span><span class="keyword">::</span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$message</span><span class="keyword">, </span><span class="default">$code</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">is_null</span><span class="keyword">(</span><span class="default">$previous</span><span class="keyword">))<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this </span><span class="keyword">-&gt; </span><span class="default">previous </span><span class="keyword">= </span><span class="default">$previous</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; public function </span><span class="default">getPrevious</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$this </span><span class="keyword">-&gt; </span><span class="default">previous</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">?&gt;<br></span><br>Hope you find it useful.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.exceptions.extending.php)

**[⬆ to root](/)**