# Collecting Cycles




<div class="phpcode"><span class="html">
Memory leak: meaning you keep a reference to it thus preventing the GC from collecting it.</span>
</div>
  

#


<div class="phpcode"><span class="html">
After testing, breaking up memory intensive code into a separate function allows the garbage collection to work.<br><br>For example the original code was like:-<br>while(true){<br>&#xA0;&#xA0; //do memory intensive code<br>}<br><br>can be turned into something like:-<br>function intensive($parameters){<br>&#xA0;&#xA0; //do memory intensive code<br>}<br><br>while(true){<br>&#xA0;&#xA0; intensive($parameters);<br>}</span>
</div>
  

#


<div class="phpcode"><span class="html">
&#x2500;&#x2500; Unused Objects &#x2500;&#x2500;&#x2500; &#x2500; In use Objects<br>&#x2193;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#x2193;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; &#x2193;<br> _____________________________________<br> |&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;|&#x2588;&#x2588;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;|<br> |&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;|&#x2588;&#x2588;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;|<br>&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#x25B2;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#x25B2;<br>&#xA0; &#xA0;&#xA0; Unreferenced&#xA0; &#xA0; &#xA0; &#xA0; Referenced<br>&#xA0; &#xA0; &#xA0;&#xA0; Objects&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; Objects<br><br>&#x2588; Memory leak</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/features.gc.collecting-cycles.php)

**[To root](/)**