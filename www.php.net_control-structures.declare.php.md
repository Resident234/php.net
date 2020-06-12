# 




<div class="phpcode"><span class="html">
It&apos;s amazing how many people didn&apos;t grasp the concept here. Note the wording in the documentation. It states that the tick handler is called every n native execution cycles. That means native instructions, not including system calls (i&apos;m guessing). This can give you a very good idea if you need to optimize a particular part of your script, since you can measure quite effectively how many native instructions are in your actual code.<br><br>A good profiler would take that into account, and force you, the developer, to include calls to the profiler as you&apos;re entering and leaving every function. That way you&apos;d be able to keep an eye on how many cycles it took each function to complete. Independent of time.<br><br>That is extremely powerful, and not to be underestimated. A good solution would allow aggregate stats, so the total time in a function would be counted, including inside called functions.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that in PHP 7 <span class="default">&lt;?php </span><span class="keyword">declare(</span><span class="default">encoding</span><span class="keyword">=</span><span class="string">&apos;...&apos;</span><span class="keyword">); </span><span class="default">?&gt;</span> throws an E_WARNING if Zend Multibyte is turned off.</span>
</div>
  

#


<div class="phpcode"><span class="html">
In the following example:<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">handler</span><span class="keyword">(){<br>&#xA0; &#xA0; print </span><span class="string">&quot;hello &lt;br /&gt;&quot;</span><span class="keyword">;<br>}<br><br></span><span class="default">register_tick_function</span><span class="keyword">(</span><span class="string">&quot;handler&quot;</span><span class="keyword">);<br><br>declare(</span><span class="default">ticks </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">){<br>&#xA0; &#xA0; </span><span class="default">$b </span><span class="keyword">= </span><span class="default">2</span><span class="keyword">;<br>} </span><span class="comment">//closing curly bracket tickable<br></span><span class="default">?&gt;<br></span><br>&quot;Hello&quot; will be displayed twice because the closing curly bracket is also tickable. <br><br>One may wonder why the opening curly bracket is not tickable if the closing is tickable. This is because the instruction for PHP to start ticking is given by the opening curly bracket so the ticking starts immediately after it.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.declare.php)

**[To root](/)**