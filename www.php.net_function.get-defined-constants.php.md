# get_defined_constants




<div class="phpcode"><span class="html">
Add this method to your class definition if you want an array of class constants (get_defined_constants doesn&apos;t work with class constants as Peter P said above):<br><br><span class="default">&lt;?php<br></span><span class="keyword">public function </span><span class="default">get_class_constants</span><span class="keyword">()<br>{<br>&#xA0; &#xA0; </span><span class="default">$reflect </span><span class="keyword">= new </span><span class="default">ReflectionClass</span><span class="keyword">(</span><span class="default">get_class</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">));<br>&#xA0; &#xA0; return </span><span class="default">$reflect</span><span class="keyword">-&gt;</span><span class="default">getConstants</span><span class="keyword">());<br>}<br></span><span class="default">?&gt;<br></span><br>You could also override stdObject with it so that all your classes&#xA0; have this method</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.get-defined-constants.php)

**[To root](/)**