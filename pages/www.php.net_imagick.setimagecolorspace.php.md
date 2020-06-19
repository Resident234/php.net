# Imagick::setImageColorspace




<div class="phpcode"><span class="html">
When converting from CMYK to RGB using this function, the image can become inverted. To fix this, use a workaround (don&apos;t forget to download the .icc files online): 
<br>
<br><span class="default">&lt;?php 
<br></span><span class="comment">// don&apos;t use this (it inverts the image) 
<br>//&#xA0; &#xA0; $img-&gt;setImageColorspace (imagick::COLORSPACE_RGB); 
<br>
<br></span><span class="keyword">if (</span><span class="default">$img</span><span class="keyword">-&gt;</span><span class="default">getImageColorspace</span><span class="keyword">() == </span><span class="default">Imagick</span><span class="keyword">::</span><span class="default">COLORSPACE_CMYK</span><span class="keyword">) { 
<br>&#xA0;&#xA0; </span><span class="default">$profiles </span><span class="keyword">= </span><span class="default">$img</span><span class="keyword">-&gt;</span><span class="default">getImageProfiles</span><span class="keyword">(</span><span class="string">&apos;*&apos;</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">); 
<br>&#xA0;&#xA0; </span><span class="comment">// we&apos;re only interested if ICC profile(s) exist 
<br>&#xA0;&#xA0; </span><span class="default">$has_icc_profile </span><span class="keyword">= (</span><span class="default">array_search</span><span class="keyword">(</span><span class="string">&apos;icc&apos;</span><span class="keyword">, </span><span class="default">$profiles</span><span class="keyword">) !== </span><span class="default">false</span><span class="keyword">); 
<br>&#xA0;&#xA0; </span><span class="comment">// if it doesnt have a CMYK ICC profile, we add one 
<br>&#xA0;&#xA0; </span><span class="keyword">if (</span><span class="default">$has_icc_profile </span><span class="keyword">=== </span><span class="default">false</span><span class="keyword">) { 
<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$icc_cmyk </span><span class="keyword">= </span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="default">dirname</span><span class="keyword">(</span><span class="default">__FILE__</span><span class="keyword">).</span><span class="string">&apos;/USWebUncoated.icc&apos;</span><span class="keyword">); 
<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$img</span><span class="keyword">-&gt;</span><span class="default">profileImage</span><span class="keyword">(</span><span class="string">&apos;icc&apos;</span><span class="keyword">, </span><span class="default">$icc_cmyk</span><span class="keyword">); 
<br>&#xA0; &#xA0; &#xA0;&#xA0; unset(</span><span class="default">$icc_cmyk</span><span class="keyword">); 
<br>&#xA0;&#xA0; } 
<br>&#xA0;&#xA0; </span><span class="comment">// then we add an RGB profile 
<br>&#xA0;&#xA0; </span><span class="default">$icc_rgb </span><span class="keyword">= </span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="default">dirname</span><span class="keyword">(</span><span class="default">__FILE__</span><span class="keyword">).</span><span class="string">&apos;/sRGB_v4_ICC_preference.icc&apos;</span><span class="keyword">); 
<br>&#xA0;&#xA0; </span><span class="default">$img</span><span class="keyword">-&gt;</span><span class="default">profileImage</span><span class="keyword">(</span><span class="string">&apos;icc&apos;</span><span class="keyword">, </span><span class="default">$icc_rgb</span><span class="keyword">); 
<br>&#xA0;&#xA0; unset(</span><span class="default">$icc_rgb</span><span class="keyword">); 
<br>} 
<br>
<br></span><span class="default">$img</span><span class="keyword">-&gt;</span><span class="default">stripImage </span><span class="keyword">(); </span><span class="comment">// this will drop down the size of the image dramatically (removes all profiles) 
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.setimagecolorspace.php)

**[To root](/README.md)**