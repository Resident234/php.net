# Debugging in PHP




<div class="phpcode"><span class="html">
I find it very useful to print out to the browsers console instead of just var_dumping:<br><br>function console_log( $data ){<br>&#xA0; echo &apos;&lt;script&gt;&apos;;<br>&#xA0; echo &apos;console.log(&apos;. json_encode( $data ) .&apos;)&apos;;<br>&#xA0; echo &apos;&lt;/script&gt;&apos;;<br>}<br><br>Usage:<br>$myvar = array(1,2,3);<br>console_log( $myvar ); // [1,2,3]</span>
</div>
  

#


<div class="phpcode"><span class="html">
I would like to add <a href="http://phpdebugbar.com/" rel="nofollow" target="_blank">http://phpdebugbar.com/</a> which is also a handy tool for debugging/profiling data. and you could use it whenever framework you are using. :-)</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/debugger.php)

**[⬆ to root](/)**