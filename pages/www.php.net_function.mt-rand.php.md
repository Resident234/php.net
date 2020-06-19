# mt_rand




<div class="phpcode"><span class="html">
i wanted to spot out the big difference between rand and mt_rand when producing images using randomness as noise.
<br>
<br>for example this is a comparation between rand and mt_rand on a 400x400 pixel png: <a href="http://oi43.tinypic.com/vwtppl.jpg" rel="nofollow" target="_blank">http://oi43.tinypic.com/vwtppl.jpg</a>
<br>
<br>code:
<br><span class="default">&lt;?php
<br>header</span><span class="keyword">(</span><span class="string">&quot;Content-type: image/png&quot;</span><span class="keyword">);
<br></span><span class="default">$sizex</span><span class="keyword">=</span><span class="default">800</span><span class="keyword">;
<br></span><span class="default">$sizey</span><span class="keyword">=</span><span class="default">400</span><span class="keyword">;
<br>
<br></span><span class="default">$img </span><span class="keyword">= </span><span class="default">imagecreatetruecolor</span><span class="keyword">(</span><span class="default">$sizex</span><span class="keyword">,</span><span class="default">$sizey</span><span class="keyword">);
<br></span><span class="default">$ink </span><span class="keyword">= </span><span class="default">imagecolorallocate</span><span class="keyword">(</span><span class="default">$img</span><span class="keyword">,</span><span class="default">255</span><span class="keyword">,</span><span class="default">255</span><span class="keyword">,</span><span class="default">255</span><span class="keyword">);
<br>
<br>for(</span><span class="default">$i</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">;</span><span class="default">$i</span><span class="keyword">&lt;</span><span class="default">$sizex</span><span class="keyword">/</span><span class="default">2</span><span class="keyword">;</span><span class="default">$i</span><span class="keyword">++) {
<br>&#xA0; for(</span><span class="default">$j</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">;</span><span class="default">$j</span><span class="keyword">&lt;</span><span class="default">$sizey</span><span class="keyword">;</span><span class="default">$j</span><span class="keyword">++) {
<br>&#xA0; </span><span class="default">imagesetpixel</span><span class="keyword">(</span><span class="default">$img</span><span class="keyword">, </span><span class="default">rand</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">,</span><span class="default">$sizex</span><span class="keyword">/</span><span class="default">2</span><span class="keyword">), </span><span class="default">rand</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">,</span><span class="default">$sizey</span><span class="keyword">), </span><span class="default">$ink</span><span class="keyword">);
<br>&#xA0; }
<br>}
<br> 
<br>for(</span><span class="default">$i</span><span class="keyword">=</span><span class="default">$sizex</span><span class="keyword">/</span><span class="default">2</span><span class="keyword">;</span><span class="default">$i</span><span class="keyword">&lt;</span><span class="default">$sizex</span><span class="keyword">;</span><span class="default">$i</span><span class="keyword">++) {
<br>&#xA0; for(</span><span class="default">$j</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">;</span><span class="default">$j</span><span class="keyword">&lt;</span><span class="default">$sizey</span><span class="keyword">;</span><span class="default">$j</span><span class="keyword">++) {
<br>&#xA0; </span><span class="default">imagesetpixel</span><span class="keyword">(</span><span class="default">$img</span><span class="keyword">, </span><span class="default">mt_rand</span><span class="keyword">(</span><span class="default">$sizex</span><span class="keyword">/</span><span class="default">2</span><span class="keyword">,</span><span class="default">$sizex</span><span class="keyword">), </span><span class="default">mt_rand</span><span class="keyword">(</span><span class="default">1</span><span class="keyword">,</span><span class="default">$sizey</span><span class="keyword">), </span><span class="default">$ink</span><span class="keyword">);
<br>&#xA0; }
<br>}
<br>
<br></span><span class="default">imagepng</span><span class="keyword">(</span><span class="default">$img</span><span class="keyword">);
<br></span><span class="default">imagedestroy</span><span class="keyword">(</span><span class="default">$img</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>the differences reduce when reducing the pixels of the image.. infact for a 100x100 pixel image the noise produced from the rand function is much more realistic than how it is for a 400x400 image: <a href="http://oi39.tinypic.com/5k0row.jpg" rel="nofollow" target="_blank">http://oi39.tinypic.com/5k0row.jpg</a>
<br>
<br>(rand is on the left, mt_rand on the right)</span>
</div>
  

#


<div class="phpcode"><span class="html">
To quickly build a human-readable random string for a captcha per example :<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">random</span><span class="keyword">(</span><span class="default">$length </span><span class="keyword">= </span><span class="default">8</span><span class="keyword">)<br>{&#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="default">$chars </span><span class="keyword">= </span><span class="string">&apos;bcdfghjklmnprstvwxzaeiou&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; for (</span><span class="default">$p </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$p </span><span class="keyword">&lt; </span><span class="default">$length</span><span class="keyword">; </span><span class="default">$p</span><span class="keyword">++)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">.= (</span><span class="default">$p</span><span class="keyword">%</span><span class="default">2</span><span class="keyword">) ? </span><span class="default">$chars</span><span class="keyword">[</span><span class="default">mt_rand</span><span class="keyword">(</span><span class="default">19</span><span class="keyword">, </span><span class="default">23</span><span class="keyword">)] : </span><span class="default">$chars</span><span class="keyword">[</span><span class="default">mt_rand</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">, </span><span class="default">18</span><span class="keyword">)];<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; return </span><span class="default">$result</span><span class="keyword">;<br>}<br><br></span><span class="default">?&gt;<br></span><br>Note that I have removed q and y from $chars to avoid readability problems.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.mt-rand.php)

**[To root](/README.md)**