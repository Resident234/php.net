# ImagickDraw::annotation





may help someone...





```
<?php

&#xA0; &#xA0; /**

&#xA0; &#xA0;&#xA0; * Split the given text into rows fitting the given maxWidth

&#xA0; &#xA0;&#xA0; *

&#xA0; &#xA0;&#xA0; * @param unknown_type $draw

&#xA0; &#xA0;&#xA0; * @param unknown_type $text

&#xA0; &#xA0;&#xA0; * @param unknown_type $maxWidth

&#xA0; &#xA0;&#xA0; * @return array

&#xA0; &#xA0;&#xA0; */

&#xA0; &#xA0; private function getTextRows($draw, $text, $maxWidth)

&#xA0; &#xA0; {&#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; $words = explode(&quot; &quot;, $text);

&#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; $lines = array();

&#xA0; &#xA0; &#xA0; &#xA0; $i=0;

&#xA0; &#xA0; &#xA0; &#xA0; while ($i &lt; count($words)) 

&#xA0; &#xA0; &#xA0; &#xA0; {//as long as there are words



&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $line = &quot;&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; do

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {//append words to line until the fit in size

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($line != &quot;&quot;){

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $line .= &quot; &quot;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $line .= $words[$i];

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $i++;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(($i) == count($words)){

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;//last word -&gt; break

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //messure size of line + next word

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $linePreview = $line.&quot; &quot;.$words[$i];

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $metrics = $this-&gt;canvas-&gt;queryFontMetrics($draw, $linePreview);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //echo $line.&quot;($i)&quot;.$metrics[&quot;textWidth&quot;].&quot;:&quot;.$maxWidth.&quot;&lt;br&gt;&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }while($metrics[&quot;textWidth&quot;] &lt;= $maxWidth);

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; //echo &quot;&lt;hr&gt;&quot;.$line.&quot;&lt;br&gt;&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $lines[] = $line;

&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; 

&#xA0; &#xA0; &#xA0; &#xA0; //var_export($lines);

&#xA0; &#xA0; &#xA0; &#xA0; return $lines;

&#xA0; &#xA0; }

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/imagickdraw.annotation.php)

**[To root](/README.md)**