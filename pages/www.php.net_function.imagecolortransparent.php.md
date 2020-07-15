# imagecolortransparent



I&apos;ve made a very simple script that will retain transparency of images especially when resizing.<br><br>NOTE: Transparency is only supported on GIF and PNG files.<br><br>Parameters:<br><br>$new_image = image resource identifier such as returned by imagecreatetruecolor(). must be passed by reference<br>$image_source = image resource identifier returned by imagecreatefromjpeg, imagecreatefromgif and imagecreatefrompng. must be passed by reference<br><br>

```
<?php
function setTransparency($new_image,$image_source)
    {
        
            $transparencyIndex = imagecolortransparent($image_source);
            $transparencyColor = array('red' => 255, 'green' => 255, 'blue' => 255);
             
            if ($transparencyIndex &gt;= 0) {
                $transparencyColor    = imagecolorsforindex($image_source, $transparencyIndex);    
            }
            
            $transparencyIndex    = imagecolorallocate($new_image, $transparencyColor['red'], $transparencyColor['green'], $transparencyColor['blue']);
            imagefill($new_image, 0, 0, $transparencyIndex);
             imagecolortransparent($new_image, $transparencyIndex);
        
    } 
?>
```



Sample Usage: (resizing)



```
<?php
$image_source = imagecreatefrompng('test.png');
$new_image = imagecreatetruecolor($width, $height);
setTransparency($new_image,$image_source);
imagecopyresampled($new_image, $image_source, 0, 0, 0, 0, $new_width, $new_height, $old_width, $old_height);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecolortransparent.php)

**[To root](/README.md)**