# Basic usage




<div class="phpcode"><span class="html">
Be careful when loading multiple images by passing an array to a new Imagick object. This is from Example #2:<br><br><span class="default">&lt;?php<br><br>$images </span><span class="keyword">= new </span><span class="default">Imagick</span><span class="keyword">(</span><span class="default">glob</span><span class="keyword">(</span><span class="string">&apos;images/*.JPG&apos;</span><span class="keyword">));<br><br></span><span class="default">?&gt;<br></span><br>If you have lots of images inside the images folder, PHP will consume a lot of memory, ergo it is not recommended. I personally think it&apos;s a better idea to loop each image separately:<br><br><span class="default">&lt;?php<br><br>$image_files </span><span class="keyword">= </span><span class="default">glob</span><span class="keyword">(</span><span class="string">&apos;images/*.JPG&apos;</span><span class="keyword">);<br><br>foreach (</span><span class="default">$image_files </span><span class="keyword">as </span><span class="default">$image_file</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$image </span><span class="keyword">= new </span><span class="default">Imagick</span><span class="keyword">(</span><span class="default">$image_file</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="comment">// Do something for the image and so on...<br></span><span class="keyword">}<br><br></span><span class="default">?&gt;<br></span><br>This way only a single image is fitted into the memory at a time.</span>
</div>
  

#


<div class="phpcode"><span class="html">
on Example #3 Creating a reflection of an image<br>----------------------------------------------------<br>/* Clone the image and flip it */<br>$reflection = $im-&gt;clone();<br>$reflection-&gt;flipImage();<br>----------------------------------------------------<br>it was using the Imagick::clone function<br><br>This function has been DEPRECATED as of imagick 3.1.0 in favour of using the clone keyword.<br><br>use below code instead:<br>----------------------------------------------------<br>/* Clone the image and flip it */<br>$reflection = clone $im;<br>$reflection-&gt;flipImage();<br>----------------------------------------------------<br><br><a href="http://php.net/manual/en/imagick.clone.php" rel="nofollow" target="_blank">http://php.net/manual/en/imagick.clone.php</a></span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.examples-1.php)

**[⬆ to root](/)**