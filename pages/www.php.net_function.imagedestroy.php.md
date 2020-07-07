# imagedestroy





When the script stop PHP will automatic destory ANY
resources, this goes for aswell for images, thus in the
case the use clicks the stop button, php will automatic
clear the resource.

thus imagedestroy is used to clear the memory BEFORE
the script ends. this is usefull to keep memory usage
DURING the script to an acceptable level.

hope this clear somethings up.

  

#



Caution should be observed with imagedestroy(); copying your reference variable over to another variable will cause imagedestroy to destroy both at once.

Eg.: 

$a = imagecreate(...);
$b = $a;
imagedestroy($a);

While you&apos;d think $b still contains your image, it doesn&apos;t. Both $a and $b are destroyed.

  

#

[Official documentation page](https://www.php.net/manual/en/function.imagedestroy.php)

**[To root](/README.md)**