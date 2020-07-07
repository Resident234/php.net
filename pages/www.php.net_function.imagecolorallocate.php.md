# imagecolorallocate





Note that you can only assign 255 colors to any image palette.&#xA0; If you try assigning more, imagecolorallocate() will fail.

If, for example, you are randomly allocating colors, it will be wise to check if you have used up all of the colors possible.&#xA0; You can use imagecolorclosest() to get the closest assigned color:


```
<?php
&#xA0; &#xA0;&#xA0; //assign random rgb values
&#xA0; &#xA0;&#xA0; $c1 = mt_rand(50,200); //r(ed)
&#xA0; &#xA0;&#xA0; $c2 = mt_rand(50,200); //g(reen)
&#xA0; &#xA0;&#xA0; $c3 = mt_rand(50,200); //b(lue)
&#xA0; &#xA0;&#xA0; //test if we have used up palette
&#xA0; &#xA0;&#xA0; if(imagecolorstotal($pic)&gt;=255) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //palette used up; pick closest assigned color
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $color = imagecolorclosest($pic, $c1, $c2, $c3);
&#xA0; &#xA0;&#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //palette NOT used up; assign new color
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $color = imagecolorallocate($pic, $c1, $c2, $c3);
&#xA0; &#xA0;&#xA0; }
?>
```


Also, imagecolorallocate() will assign a new color EVERY time the function is called, even if the color already exists in the palette:


```
<?php
&#xA0; &#xA0;&#xA0; // [...]
&#xA0; &#xA0;&#xA0; imagecolorallocate($pic,125,125,125); //returns 5
&#xA0; &#xA0;&#xA0; imagecolorallocate($pic,125,125,125); //returns 6
&#xA0; &#xA0;&#xA0; imagecolorallocate($pic,125,125,125); //returns 7
&#xA0; &#xA0;&#xA0; // [...]
&#xA0; &#xA0;&#xA0; imagecolorallocate($pic,125,125,125); //returns 23
&#xA0; &#xA0;&#xA0; imagecolorallocate($pic,125,125,125); //returns 25
&#xA0; &#xA0;&#xA0; // [...]
&#xA0; &#xA0;&#xA0; // etc...
?>
```


So here, imagecolorexact() is useful:


```
<?php
&#xA0; &#xA0;&#xA0; //see if color already exists
&#xA0; &#xA0;&#xA0; $color = imagecolorexact($pic, $c1, $c2, $c3);
&#xA0; &#xA0;&#xA0; if($color==-1) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //color does not exist; allocate a new one
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $color = imagecolorallocate($pic, $c1, $c2, $c3);
&#xA0; &#xA0;&#xA0; }
?>
```


And, for nerdy-ness sake, we can put the two ideas together:


```
<?php
&#xA0; &#xA0;&#xA0; //assign random rgb values
&#xA0; &#xA0;&#xA0; $c1 = mt_rand(50,200); //r(ed)
&#xA0; &#xA0;&#xA0; $c2 = mt_rand(50,200); //g(reen)
&#xA0; &#xA0;&#xA0; $c3 = mt_rand(50,200); //b(lue)
&#xA0; &#xA0;&#xA0; //get color from palette
&#xA0; &#xA0;&#xA0; $color = imagecolorexact($pic, $c1, $c2, $c3);
&#xA0; &#xA0;&#xA0; if($color==-1) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //color does not exist...
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //test if we have used up palette
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(imagecolorstotal($pic)&gt;=255) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; //palette used up; pick closest assigned color
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $color = imagecolorclosest($pic, $c1, $c2, $c3);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; //palette NOT used up; assign new color
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $color = imagecolorallocate($pic, $c1, $c2, $c3);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0;&#xA0; }
?>
```


Or as a function:


```
<?php
&#xA0; &#xA0;&#xA0; function createcolor($pic,$c1,$c2,$c3) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //get color from palette
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $color = imagecolorexact($pic, $c1, $c2, $c3);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($color==-1) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; //color does not exist...
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; //test if we have used up palette
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; if(imagecolorstotal($pic)&gt;=255) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //palette used up; pick closest assigned color
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $color = imagecolorclosest($pic, $c1, $c2, $c3);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //palette NOT used up; assign new color
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $color = imagecolorallocate($pic, $c1, $c2, $c3);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $color;
&#xA0; &#xA0;&#xA0; }

&#xA0; &#xA0;&#xA0; for($i=0; $i&lt;1000; $i++) { //1000 because it is significantly greater than 255
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //assign random rgb values
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $c1 = mt_rand(50,200); //r(ed)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $c2 = mt_rand(50,200); //g(reen)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $c3 = mt_rand(50,200); //b(lue)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //generate the color
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $color = createcolor($pic,$c1,$c2,$c3);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //do something with color...
&#xA0; &#xA0;&#xA0; }
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecolorallocate.php)

**[To root](/README.md)**