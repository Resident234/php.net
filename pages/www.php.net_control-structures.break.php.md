# break




<div class="phpcode"><span class="html">
A break statement that is in the outer part of a program (e.g. not in a control loop) will end the script. This caught me out when I mistakenly had a break in an if statement<br><br>i.e.<br><br><span class="default">&lt;?php <br></span><span class="keyword">echo </span><span class="string">&quot;hello&quot;</span><span class="keyword">;<br>if (</span><span class="default">true</span><span class="keyword">) break;<br>echo </span><span class="string">&quot; world&quot;</span><span class="keyword">; <br></span><span class="default">?&gt;<br></span><br>will only show &quot;hello&quot;</span>
</div>
  

#


<div class="phpcode"><span class="html">
vlad at vlad dot neosurge dot net wrote on 04-Jan-2003 04:21<br><br>&gt; Just an insignificant side not: Like in C/C++, it&apos;s not <br>&gt; necessary to break out of the default part of a switch <br>&gt; statement in PHP.<br><br>It&apos;s not necessary to break out of any case of a switch&#xA0; statement in PHP, but if you want only one case to be executed, you have do break out of it (even out of the default case).<br><br>Consider this:<br><br><span class="default">&lt;?php<br>$a </span><span class="keyword">= </span><span class="string">&apos;Apple&apos;</span><span class="keyword">;<br>switch (</span><span class="default">$a</span><span class="keyword">) {<br>&#xA0; &#xA0; default:<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;$a is not an orange&lt;br&gt;&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; case </span><span class="string">&apos;Orange&apos;</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;$a is an orange&apos;</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>This prints (in PHP 5.0.4 on MS-Windows):<br>$a is not an orange<br>$a is an orange<br><br>Note that the PHP documentation does not state the default part must be the last case statement.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.break.php)

**[To root](/README.md)**