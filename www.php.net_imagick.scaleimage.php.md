# Imagick::scaleImage




<div class="phpcode"><span class="html">
If anyone finds &quot;The other parameter will be calculated if 0 is passed as either param. &quot; to be a bit confusing, it means approximately this:<br><br><span class="default">&lt;?php<br>$im </span><span class="keyword">= new </span><span class="default">Imagick</span><span class="keyword">(</span><span class="string">&apos;example.jpg&apos;</span><span class="keyword">);<br></span><span class="default">$im</span><span class="keyword">-&gt;</span><span class="default">scaleImage</span><span class="keyword">(</span><span class="default">300</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>This scales the image such that it is now 300 pixels wide, and automatically calculates the height to keep the image at the same aspect ratio.<br><br><span class="default">&lt;?php<br>$im </span><span class="keyword">= new </span><span class="default">Imagick</span><span class="keyword">(</span><span class="string">&apos;example.jpg&apos;</span><span class="keyword">);<br></span><span class="default">$im</span><span class="keyword">-&gt;</span><span class="default">scaleImage</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">, </span><span class="default">300</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Similarly, this example scales the image to make it 300 pixels tall, and the method automatically recalculates the image&apos;s height to maintain the aspect ratio.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.scaleimage.php)

**[⬆ to root](/)**