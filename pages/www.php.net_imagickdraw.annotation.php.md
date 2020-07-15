# ImagickDraw::annotation



may help someone...<br><br>

```
<?php
    /**
     * Split the given text into rows fitting the given maxWidth
     *
     * @param unknown_type $draw
     * @param unknown_type $text
     * @param unknown_type $maxWidth
     * @return array
     */
    private function getTextRows($draw, $text, $maxWidth)
    {        
        $words = explode(" ", $text);
        
        $lines = array();
        $i=0;
        while ($i < count($words)) 
        {//as long as there are words

            $line = "";
            do
            {//append words to line until the fit in size
                if($line != ""){
                    $line .= " ";
                }
                $line .= $words[$i];
                
                
                $i++;
                if(($i) == count($words)){
                    break;//last word -> break
                }
                
                //messure size of line + next word
                $linePreview = $line." ".$words[$i];
                $metrics = $this->canvas->queryFontMetrics($draw, $linePreview);
                //echo $line."($i)".$metrics["textWidth"].":".$maxWidth."<br>";
                
            }while($metrics["textWidth"] <= $maxWidth);
            
            //echo "<hr>".$line."<br>";
            $lines[] = $line;
        }
        
        //var_export($lines);
        return $lines;
    }
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/imagickdraw.annotation.php)

**[To root](/README.md)**