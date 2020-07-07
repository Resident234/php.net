# __halt_compiler




<div class="phpcode"><span class="html">
This function can be used in eval() -- it will halt the eval, but not the script eval&quot;() was called in.</span>
</div>
  

#


<div class="phpcode"><span class="html">
__halt_compiler is also useful for debugging. If you need to temporarily make a change that will introduce an error later on, use __halt_compiler to prevent syntax errors. For example:<br><br><span class="default">&lt;?php<br></span><span class="keyword">if ( </span><span class="default">$something </span><span class="keyword">):<br>&#xA0; print </span><span class="string">&apos;something&apos;</span><span class="keyword">;<br>endif;&#xA0;&#xA0; </span><span class="comment">// endif placed here for debugging purposes<br></span><span class="keyword">__halt_compiler();<br>endif;&#xA0;&#xA0; </span><span class="comment">// original location of endif -- would produce syntax error if __halt_compiler was not there<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.halt-compiler.php)

**[To root](/README.md)**