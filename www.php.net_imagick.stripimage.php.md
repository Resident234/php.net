# Imagick::stripImage




<div class="phpcode"><span class="html">
StripImage also delete ICC image profile by default.<br>The resulting images seem to lose a lot of color information and look &quot;flat&quot; compared to their non-stripped versions.<br><br>Consider keeping the ICC profile (which causes richer colors) while removing all other EXIF data:<br><br>1. Extract the ICC profile<br>2. Strip EXIF data and image profile<br>3. Add the ICC profile back<br><br>The code is:<br><span class="default">&lt;?php<br>$profiles </span><span class="keyword">= </span><span class="default">$img</span><span class="keyword">-&gt;</span><span class="default">getImageProfiles</span><span class="keyword">(</span><span class="string">&quot;icc&quot;</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br><br></span><span class="default">$img</span><span class="keyword">-&gt;</span><span class="default">stripImage</span><span class="keyword">();<br><br>if(!empty(</span><span class="default">$profiles</span><span class="keyword">))<br>&#xA0; &#xA0; </span><span class="default">$img</span><span class="keyword">-&gt;</span><span class="default">profileImage</span><span class="keyword">(</span><span class="string">&quot;icc&quot;</span><span class="keyword">, </span><span class="default">$profiles</span><span class="keyword">[</span><span class="string">&apos;icc&apos;</span><span class="keyword">]);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Please note that striping off the exif information without handling the orientation information available in the exif will lead to wrong orientation of the image</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.stripimage.php)

**[⬆ to root](/)**