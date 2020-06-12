# imagecreatefrompng




<div class="phpcode"><span class="html">
If you&apos;re trying to load a translucent png-24 image but are finding an absence of transparency (like it&apos;s black), you need to enable alpha channel AND save the setting. I&apos;m new to GD and it took me almost two hours to figure this out.<br><br><span class="default">&lt;?php<br>$imgPng </span><span class="keyword">= </span><span class="default">imageCreateFromPng</span><span class="keyword">(</span><span class="default">$strImagePath</span><span class="keyword">);<br></span><span class="default">imageAlphaBlending</span><span class="keyword">(</span><span class="default">$imgPng</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br></span><span class="default">imageSaveAlpha</span><span class="keyword">(</span><span class="default">$imgPng</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br><br></span><span class="comment">/* Output image to browser */<br></span><span class="default">header</span><span class="keyword">(</span><span class="string">&quot;Content-type: image/png&quot;</span><span class="keyword">);<br></span><span class="default">imagePng</span><span class="keyword">(</span><span class="default">$imgPng</span><span class="keyword">); <br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecreatefrompng.php)

**[⬆ to root](/)**