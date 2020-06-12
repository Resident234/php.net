# imagedestroy




<div class="phpcode"><span class="html">
When the script stop PHP will automatic destory ANY<br>resources, this goes for aswell for images, thus in the<br>case the use clicks the stop button, php will automatic<br>clear the resource.<br><br>thus imagedestroy is used to clear the memory BEFORE<br>the script ends. this is usefull to keep memory usage<br>DURING the script to an acceptable level.<br><br>hope this clear somethings up.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Caution should be observed with imagedestroy(); copying your reference variable over to another variable will cause imagedestroy to destroy both at once.<br><br>Eg.: <br><br>$a = imagecreate(...);<br>$b = $a;<br>imagedestroy($a);<br><br>While you&apos;d think $b still contains your image, it doesn&apos;t. Both $a and $b are destroyed.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagedestroy.php)

**[⬆ to root](/)**