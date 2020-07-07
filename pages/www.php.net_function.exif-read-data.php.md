# exif_read_data





When the new update came out from Apple for iOS6 it provided the ability for iPad, iPod, and iPhones to be able to upload files from the device through Safari. Obviously this will open up an array of implementations where at one point it was just not possible.

The issue comes when a photo is uploaded it will be dependent on the location of the &quot;button&quot; when the photo was taken. Imagine if you will that you have your iPhone turned with the button at the top and you take a photo. The photo when uploaded to your server might be &quot;upside down&quot;.

The following code will ensure that all uploaded photos will be oriented correctly upon upload:


```
<?php
$image = imagecreatefromstring(file_get_contents($_FILES[&apos;image_upload&apos;][&apos;tmp_name&apos;]));
$exif = exif_read_data($_FILES[&apos;image_upload&apos;][&apos;tmp_name&apos;]);
if(!empty($exif[&apos;Orientation&apos;])) {
&#xA0; &#xA0; switch($exif[&apos;Orientation&apos;]) {
&#xA0; &#xA0; &#xA0; &#xA0; case 8:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $image = imagerotate($image,90,0);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; case 3:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $image = imagerotate($image,180,0);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; case 6:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $image = imagerotate($image,-90,0);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; }
}
// $image now contains a resource with the image oriented correctly
?>
```


What you do with the image resource from there is entirely up to you.

I hope that this helps you identify and orient any image that&apos;s uploaded from an iPad, iPhone, or iPod. Orientation for the photo is the key to knowing how to rotate it correctly.

  

#

[Official documentation page](https://www.php.net/manual/en/function.exif-read-data.php)

**[To root](/README.md)**