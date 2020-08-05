# imagecolorallocate



Note that you can only assign 255 colors to any image palette.  If you try assigning more, imagecolorallocate() will fail.<br><br>If, for example, you are randomly allocating colors, it will be wise to check if you have used up all of the colors possible.  You can use imagecolorclosest() to get the closest assigned color:<br>

```
<?php
     //assign random rgb values
     $c1 = mt_rand(50,200); //r(ed)
     $c2 = mt_rand(50,200); //g(reen)
     $c3 = mt_rand(50,200); //b(lue)
     //test if we have used up palette
     if(imagecolorstotal($pic)>=255) {
          //palette used up; pick closest assigned color
          $color = imagecolorclosest($pic, $c1, $c2, $c3);
     } else {
          //palette NOT used up; assign new color
          $color = imagecolorallocate($pic, $c1, $c2, $c3);
     }
?>
```


Also, imagecolorallocate() will assign a new color EVERY time the function is called, even if the color already exists in the palette:


```
<?php
     // [...]
     imagecolorallocate($pic,125,125,125); //returns 5
     imagecolorallocate($pic,125,125,125); //returns 6
     imagecolorallocate($pic,125,125,125); //returns 7
     // [...]
     imagecolorallocate($pic,125,125,125); //returns 23
     imagecolorallocate($pic,125,125,125); //returns 25
     // [...]
     // etc...
?>
```


So here, imagecolorexact() is useful:


```
<?php
     //see if color already exists
     $color = imagecolorexact($pic, $c1, $c2, $c3);
     if($color==-1) {
          //color does not exist; allocate a new one
          $color = imagecolorallocate($pic, $c1, $c2, $c3);
     }
?>
```


And, for nerdy-ness sake, we can put the two ideas together:


```
<?php
     //assign random rgb values
     $c1 = mt_rand(50,200); //r(ed)
     $c2 = mt_rand(50,200); //g(reen)
     $c3 = mt_rand(50,200); //b(lue)
     //get color from palette
     $color = imagecolorexact($pic, $c1, $c2, $c3);
     if($color==-1) {
          //color does not exist...
          //test if we have used up palette
          if(imagecolorstotal($pic)>=255) {
               //palette used up; pick closest assigned color
               $color = imagecolorclosest($pic, $c1, $c2, $c3);
          } else {
               //palette NOT used up; assign new color
               $color = imagecolorallocate($pic, $c1, $c2, $c3);
          }
     }
?>
```


Or as a function:


```
<?php
     function createcolor($pic,$c1,$c2,$c3) {
          //get color from palette
          $color = imagecolorexact($pic, $c1, $c2, $c3);
          if($color==-1) {
               //color does not exist...
               //test if we have used up palette
               if(imagecolorstotal($pic)>=255) {
                    //palette used up; pick closest assigned color
                    $color = imagecolorclosest($pic, $c1, $c2, $c3);
               } else {
                    //palette NOT used up; assign new color
                    $color = imagecolorallocate($pic, $c1, $c2, $c3);
               }
          }
          return $color;
     }

     for($i=0; $i<1000; $i++) { //1000 because it is significantly greater than 255
          //assign random rgb values
          $c1 = mt_rand(50,200); //r(ed)
          $c2 = mt_rand(50,200); //g(reen)
          $c3 = mt_rand(50,200); //b(lue)
          //generate the color
          $color = createcolor($pic,$c1,$c2,$c3);
          //do something with color...
     }
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.imagecolorallocate.php)

**[To root](/README.md)**