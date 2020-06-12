# Instruction separation




<div class="phpcode"><span class="html">
Do not mis interpret<br><br><span class="default">&lt;?php </span><span class="keyword">echo </span><span class="string">&apos;Ending tag excluded&apos;</span><span class="keyword">; <br><br></span><span class="default">with<br><br></span><span class="keyword">&lt;?</span><span class="default">php </span><span class="keyword">echo </span><span class="string">&apos;Ending tag excluded&apos;</span><span class="keyword">;<br>&lt;</span><span class="default">p</span><span class="keyword">&gt;</span><span class="default">But html is still visible</span><span class="keyword">&lt;/</span><span class="default">p</span><span class="keyword">&gt;<br><br></span><span class="default">The second one would give error</span><span class="keyword">. </span><span class="default">Exclude ?&gt;</span> if you no more html to write after the code.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.basic-syntax.instruction-separation.php)

**[To root](/)**