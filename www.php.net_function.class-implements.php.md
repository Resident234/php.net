# class_implements




<div class="phpcode"><span class="html">
You can also check if a class implements an interface using instanceof.<br><br>E.g.<br><span class="default">&lt;?php<br></span><span class="keyword">if(</span><span class="default">$myObj </span><span class="keyword">instanceof </span><span class="default">MyInterface</span><span class="keyword">) {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;It is! It is!&quot;</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Hint:<br><span class="default">&lt;?php<br>in_array</span><span class="keyword">(</span><span class="string">&quot;your-interface&quot;</span><span class="keyword">, </span><span class="default">class_implements</span><span class="keyword">(</span><span class="default">$object_or_class_name</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span>would check if &apos;your-interface&apos; is ONE of the implemented interfaces.<br>Note that you can use something similar to be sure the class only implements that, (whyever you would want that?)<br><span class="default">&lt;?php<br></span><span class="keyword">array(</span><span class="string">&quot;your-interface&quot;</span><span class="keyword">) == </span><span class="default">class_implements</span><span class="keyword">(</span><span class="default">$object_or_class_name</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>I use the first technique to check if a module has the correct interface implemented, or else it throws an exception.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.class-implements.php)

**[⬆ to root](/)**