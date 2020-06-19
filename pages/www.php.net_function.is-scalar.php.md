# is_scalar




<div class="phpcode"><span class="html">
Having hunted around the manual, I&apos;ve not found a clear statement of what makes a type &quot;scalar&quot; (e.g. if some future version of the language introduces a new kind of type, what criterion will decide if it&apos;s &quot;scalar&quot;? - that goes beyond just listing what&apos;s scalar in the current version.)<br><br>In other lanuages, it means &quot;has ordering operators&quot; - i.e. &quot;less than&quot; and friends.<br><br>It (-:currently:-) appears to have the same meaning in PHP.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Another warning in response to the previous note:<br>&gt; just a warning as it appears that an empty value is not a scalar.<br><br>That statement is wrong--or, at least, has been fixed with a later revision than the one tested.&#xA0; The following code generated the following output on PHP 4.3.9.<br><br>CODE:<br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">echo(</span><span class="string">&apos;is_scalar() test:&apos;</span><span class="keyword">.</span><span class="default">EOL</span><span class="keyword">);<br>&#xA0; &#xA0; echo(</span><span class="string">&quot;NULL: &quot;&#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="default">print_R</span><span class="keyword">(</span><span class="default">is_scalar</span><span class="keyword">(</span><span class="default">NULL</span><span class="keyword">),&#xA0; &#xA0;&#xA0; </span><span class="default">true</span><span class="keyword">) . </span><span class="default">EOL</span><span class="keyword">);<br>&#xA0; &#xA0; echo(</span><span class="string">&quot;false: &quot;&#xA0; &#xA0; </span><span class="keyword">. </span><span class="default">print_R</span><span class="keyword">(</span><span class="default">is_scalar</span><span class="keyword">(</span><span class="default">false</span><span class="keyword">),&#xA0;&#xA0; </span><span class="default">true</span><span class="keyword">) . </span><span class="default">EOL</span><span class="keyword">);<br>&#xA0; &#xA0; echo(</span><span class="string">&quot;(empty): &quot;&#xA0; </span><span class="keyword">. </span><span class="default">print_R</span><span class="keyword">(</span><span class="default">is_scalar</span><span class="keyword">(</span><span class="string">&apos;&apos;</span><span class="keyword">),&#xA0; &#xA0; &#xA0; </span><span class="default">true</span><span class="keyword">) . </span><span class="default">EOL</span><span class="keyword">);<br>&#xA0; &#xA0; echo(</span><span class="string">&quot;0: &quot;&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">. </span><span class="default">print_R</span><span class="keyword">(</span><span class="default">is_scalar</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">),&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">true</span><span class="keyword">) . </span><span class="default">EOL</span><span class="keyword">);<br>&#xA0; &#xA0; echo(</span><span class="string">&quot;&apos;0&apos;: &quot;&#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="default">print_R</span><span class="keyword">(</span><span class="default">is_scalar</span><span class="keyword">(</span><span class="string">&apos;0&apos;</span><span class="keyword">),&#xA0; &#xA0;&#xA0; </span><span class="default">true</span><span class="keyword">) . </span><span class="default">EOL</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>OUTPUT:<br>is_scalar() test:<br>NULL: <br>false: 1<br>(empty): 1<br>0: 1<br>&apos;0&apos;: 1<br><br>THUS:<br>&#xA0;&#xA0; * NULL is NOT a scalar<br>&#xA0;&#xA0; * false, (empty string), 0, and &quot;0&quot; ARE scalars</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.is-scalar.php)

**[To root](/README.md)**