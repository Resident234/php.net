# is_subclass_of




<div class="phpcode"><span class="html">
is_subclass_of() works also with classes between the class of obj and the superclass.<br><br>example:<br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">A </span><span class="keyword">{};<br>class </span><span class="default">B </span><span class="keyword">extends </span><span class="default">A </span><span class="keyword">{};<br>class </span><span class="default">C </span><span class="keyword">extends </span><span class="default">B </span><span class="keyword">{};<br><br></span><span class="default">$foo</span><span class="keyword">=new </span><span class="default">C</span><span class="keyword">();<br>echo ((</span><span class="default">is_subclass_of</span><span class="keyword">(</span><span class="default">$foo</span><span class="keyword">,</span><span class="string">&apos;A&apos;</span><span class="keyword">)) ? </span><span class="string">&apos;true&apos; </span><span class="keyword">: </span><span class="string">&apos;false&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>echoes &apos;true&apos; .</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.is-subclass-of.php)

**[⬆ to root](/)**