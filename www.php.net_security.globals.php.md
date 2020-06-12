# Using Register Globals




<div class="phpcode"><span class="html">
It would make this whole issue a lot less confusing for less-experienced PHP programmers if you just explained:<br><br>- $myVariable no longer works by default<br>- $_GET[&apos;myVariable&apos;] works just fine<br><br>I&apos;m embarrassed to say it&apos;s taken me six months since my ISP upgraded to PHP5 figure this out.&#xA0; I&apos;ve completely rewritten scripts to stop using GET variables altogether.<br><br>I&apos;m dumb.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Beware that all the solutions given in the comments below for emulating register_global being off are bogus, because they can destroy predefined variables you should not unset. For example, suppose that you have<br><br><span class="default">&lt;?php $_GET</span><span class="keyword">[</span><span class="string">&apos;_COOKIE&apos;</span><span class="keyword">] == </span><span class="string">&apos;foo&apos;</span><span class="keyword">; </span><span class="default">?&gt;<br></span><br>Then the simplistic solutions of the previous comments let you lose all the cookies registered in the superglobal &quot;$_COOKIE&quot;! (Note that in this situation, even with register_global set to &quot;on&quot;, PHP is smart enough to not mess predefined variables such as&#xA0; $_COOKIE.)<br><br>A proper solution for emulating register_global being off is given in the FAQ, as stated in the documentation above.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/security.globals.php)

**[⬆ to root](/)**