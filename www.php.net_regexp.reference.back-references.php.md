# Back references




<div class="phpcode"><span class="html">
Something similar opportunity is DEFINE.<br><br>Example:<br>&#xA0; &#xA0; (?(DEFINE)(?&lt;myname&gt;\bvery\b))(?&amp;myname)\p{Pd}(?&amp;myname).<br><br>Expression above will match &quot;very-very&quot; from next sentence:<br>&#xA0; &#xA0; Define is very-very handy sometimes.<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ^-------^<br><br>How it works. (?(DEFINE)(?&lt;myname&gt;\bvery\b)) - this block defines &quot;myname&quot; equal to &quot;\bvery\b&quot;. So, this block &quot;(?&amp;myname)\p{Pd}(?&amp;myname)&quot; equvivalent to &quot;\bvery\b\p{Pd}\bvery\b&quot;.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/regexp.reference.back-references.php)

**[⬆ to root](/)**