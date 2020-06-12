# ReflectionClass::getParentClass




<div class="phpcode"><span class="html">
Quick correction, the code for getting all parent classes below has a &quot;typo&quot;, you need to reset the $class variable to the parent class instance, otherwise it just endlessly loops:<br><br>&#xA0; &#xA0; &#xA0; &#xA0; $class = new ReflectionClass(&apos;classname&apos;);<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; $parents = array();<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; while ($parent = $class-&gt;getParentClass()) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $parents[] = $parent-&gt;getName();<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $class = $parent;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; echo &quot;Parents: &quot; . implode(&quot;, &quot;, $parents);</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reflectionclass.getparentclass.php)

**[⬆ to root](/)**