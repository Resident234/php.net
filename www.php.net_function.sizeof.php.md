# sizeof




<div class="phpcode"><span class="html">
I am quite surprised about previous posts. Here are my advices:<br><br>1/ prefer the count() function instead of sizeOf() as sizeOf() is only an alias of count() and does not mean the same in many other languages based on C (avoid ambiguity).<br><br>2/ prefer the powerful forEach() function to iterate over arrays.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I would recommend not using sizeof(). Many programmers expect sizeof() to return the amount of memory allocated. Instead sizeof() -as described above- is an alias for count().<br><br>Prevent misinterpretation and use count() instead.</span>
</div>
  

#


<div class="phpcode"><span class="html">
a) Always try and use PHP&apos;s internal routines to iterate through objects of various types (arrays in most examples below).<br><br>Instead of interpreting your code to loop through them, they use their own internal routines which are much faster.<br><br>(This is why foreach () will run faster than manual interation)<br><br>b) It is _always_ good practice to leave as many static resulting functions outside of loops, having operations that return the exact same piece of data every iteration of the loop is not pretty on resources.<br><br>c) I agree with PixEye&apos;s remarks on sizeof().&#xA0; In PHP it is just an alias for the true function count().&#xA0; It has other meanings logically in other languages rather than the number of elements in an object.&#xA0; This should be avoided as it may confuse developers transitioning to PHP from other languages.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If your array is &quot;huge&quot;<br><br>It is reccomended to set a variable first for this case:<br><br>THIS-&gt;<br><br>$max = sizeof($huge_array);<br>for($i = 0; $i &lt; $max;$i++)<br>{<br>code...<br>}<br><br>IS QUICKER THEN-&gt;<br><br>for($i = 0; $i &lt; sizeof($huge_array);$i++)<br>{<br>code...<br>}</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.sizeof.php)

**[⬆ to root](/)**