# imagecreate



to create an image from a BMP file, I made this function, that return a resource like the others ImageCreateFrom function:<br><br>

```
<?php
/*********************************************/
/* Fonction: ImageCreateFromBMP              */
/* Author:   DHKold                          */
/* Contact:  admin@dhkold.com                */
/* Date:     The 15th of June 2005           */
/* Version:  2.0B                            */
/*********************************************/

function ImageCreateFromBMP($filename)
{
 //Ouverture du fichier en mode binaire
   if (! $f1 = fopen($filename,"rb")) return FALSE;

 //1 : Chargement des ent&#xFFFD;tes FICHIER
   $FILE = unpack("vfile_type/Vfile_size/Vreserved/Vbitmap_offset", fread($f1,14));
   if ($FILE[&apos;file_type&apos;] != 19778) return FALSE;

 //2 : Chargement des ent&#xFFFD;tes BMP
   $BMP = unpack(&apos;Vheader_size/Vwidth/Vheight/vplanes/vbits_per_pixel&apos;.
                 &apos;/Vcompression/Vsize_bitmap/Vhoriz_resolution&apos;.
                 &apos;/Vvert_resolution/Vcolors_used/Vcolors_important&apos;, fread($f1,40));
   $BMP[&apos;colors&apos;] = pow(2,$BMP[&apos;bits_per_pixel&apos;]);
   if ($BMP[&apos;size_bitmap&apos;] == 0) $BMP[&apos;size_bitmap&apos;] = $FILE[&apos;file_size&apos;] - $FILE[&apos;bitmap_offset&apos;];
   $BMP[&apos;bytes_per_pixel&apos;] = $BMP[&apos;bits_per_pixel&apos;]/8;
   $BMP[&apos;bytes_per_pixel2&apos;] = ceil($BMP[&apos;bytes_per_pixel&apos;]);
   $BMP[&apos;decal&apos;] = ($BMP[&apos;width&apos;]*$BMP[&apos;bytes_per_pixel&apos;]/4);
   $BMP[&apos;decal&apos;] -= floor($BMP[&apos;width&apos;]*$BMP[&apos;bytes_per_pixel&apos;]/4);
   $BMP[&apos;decal&apos;] = 4-(4*$BMP[&apos;decal&apos;]);
   if ($BMP[&apos;decal&apos;] == 4) $BMP[&apos;decal&apos;] = 0;

 //3 : Chargement des couleurs de la palette
   $PALETTE = array();
   if ($BMP[&apos;colors&apos;] &lt; 16777216)
   {
    $PALETTE = unpack(&apos;V&apos;.$BMP[&apos;colors&apos;], fread($f1,$BMP[&apos;colors&apos;]*4));
   }

 //4 : Cr&#xFFFD;ation de l&apos;image
   $IMG = fread($f1,$BMP[&apos;size_bitmap&apos;]);
   $VIDE = chr(0);

   $res = imagecreatetruecolor($BMP[&apos;width&apos;],$BMP[&apos;height&apos;]);
   $P = 0;
   $Y = $BMP[&apos;height&apos;]-1;
   while ($Y &gt;= 0)
   {
    $X=0;
    while ($X &lt; $BMP[&apos;width&apos;])
    {
     if ($BMP[&apos;bits_per_pixel&apos;] == 24)
        $COLOR = unpack("V",substr($IMG,$P,3).$VIDE);
     elseif ($BMP[&apos;bits_per_pixel&apos;] == 16)
     {  
        $COLOR = unpack("n",substr($IMG,$P,2));
        $COLOR[1] = $PALETTE[$COLOR[1]+1];
     }
     elseif ($BMP[&apos;bits_per_pixel&apos;] == 8)
     {  
        $COLOR = unpack("n",$VIDE.substr($IMG,$P,1));
        $COLOR[1] = $PALETTE[$COLOR[1]+1];
     }
     elseif ($BMP[&apos;bits_per_pixel&apos;] == 4)
     {
        $COLOR = unpack("n",$VIDE.substr($IMG,floor($P),1));
        if (($P*2)%2 == 0) $COLOR[1] = ($COLOR[1] &gt;&gt; 4) ; else $COLOR[1] = ($COLOR[1] &amp; 0x0F);
        $COLOR[1] = $PALETTE[$COLOR[1]+1];
     }
     elseif ($BMP[&apos;bits_per_pixel&apos;] == 1)
     {
        $COLOR = unpack("n",$VIDE.substr($IMG,floor($P),1));
        if     (($P*8)%8 == 0) $COLOR[1] =  $COLOR[1]        &gt;&gt;7;
        elseif (($P*8)%8 == 1) $COLOR[1] = ($COLOR[1] &amp; 0x40)&gt;&gt;6;
        elseif (($P*8)%8 == 2) $COLOR[1] = ($COLOR[1] &amp; 0x20)&gt;&gt;5;
        elseif (($P*8)%8 == 3) $COLOR[1] = ($COLOR[1] &amp; 0x10)&gt;&gt;4;
        elseif (($P*8)%8 == 4) $COLOR[1] = ($COLOR[1] &amp; 0x8)&gt;&gt;3;
        elseif (($P*8)%8 == 5) $COLOR[1] = ($COLOR[1] &amp; 0x4)&gt;&gt;2;
        elseif (($P*8)%8 == 6) $COLOR[1] = ($COLOR[1] &amp; 0x2)&gt;&gt;1;
        elseif (($P*8)%8 == 7) $COLOR[1] = ($COLOR[1] &amp; 0x1);
        $COLOR[1] = $PALETTE[$COLOR[1]+1];
     }
     else
        return FALSE;
     imagesetpixel($res,$X,$Y,$COLOR[1]);
     $X++;
     $P += $BMP[&apos;bytes_per_pixel&apos;];
    }
    $Y--;
    $P+=$BMP[&apos;decal&apos;];
   }

 //Fermeture du fichier
   fclose($f1);

 return $res;
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.imagecreate.php)

**[To root](/README.md)**