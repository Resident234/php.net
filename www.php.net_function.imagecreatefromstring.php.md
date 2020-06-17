# imagecreatefromstring




<div class="phpcode"><span class="html">
While downloading images from internet, it&apos;s easiest to let php decide what is the file type. So, forget using imagecreatefromjpg, imagecreatefromgif and imagecreatefrompng. Instead, this is the way to go:<br><br><span class="default">&lt;?php<br>$src </span><span class="keyword">= </span><span class="string">&quot;<a href="http://www.varuste.net/tiedostot/l_ylabanneri.jpg" rel="nofollow" target="_blank">http://www.varuste.net/tiedostot/l_ylabanneri.jpg</a>&quot;</span><span class="keyword">;<br></span><span class="default">$image </span><span class="keyword">= </span><span class="default">imagecreatefromstring</span><span class="keyword">(</span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="default">$src</span><span class="keyword">));<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
My site allows anonymous uploads to a web-accessible location (that will execute a script if it finds one).<br><br>Naturally, I need to verify that only harmless content is accepted. I am expecting only images, so I use:<br><br><span class="default">&lt;?php<br>&#xA0; $im </span><span class="keyword">= </span><span class="default">$imagecreatefromstring</span><span class="keyword">(</span><span class="default">$USERFILE</span><span class="keyword">);<br>&#xA0; </span><span class="default">$valid </span><span class="keyword">= (</span><span class="default">$im </span><span class="keyword">!= </span><span class="default">FALSE</span><span class="keyword">);<br>&#xA0; </span><span class="default">imagedestroy</span><span class="keyword">(</span><span class="default">$im</span><span class="keyword">);<br>&#xA0; return </span><span class="default">$valid</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecreatefromstring.php)

**[To root](/README.md)**