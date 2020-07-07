# opendir





Sometimes the programmer needs to access folder content which has arabic name but the opendir function will return null resources id

for that we must convert the dirname charset from utf-8 to windows-1256 by the iconv function just if the preg_match function detect arabic characters and use &quot; U &quot; additionality to enable multibyte matching



```
<?php

$dir = (&quot;./&quot;); // on this file dir
&#xA0; &#xA0; &#xA0; 
// detect if the path has arabic characters and use &quot; u &quot;&#xA0; optional to enable function to match multibyte characters

if (preg_match(&apos;#[\x{0600}-\x{06FF}]#iu&apos;, $dir) )&#xA0; 
{

&#xA0; &#xA0; // convert input ( utf-8 ) to output ( windows-1256 ) 
&#xA0; &#xA0; 
&#xA0; &#xA0; $dir = iconv(&quot;utf-8&quot;,&quot;windows-1256&quot;,$dir);
&#xA0; &#xA0; 
}

 if( is_dir($dir) ) 
 {
&#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0;&#xA0; if(&#xA0; ( $dh = opendir($dir) ) !== null&#xA0; ) 
&#xA0; &#xA0;&#xA0; {
&#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; while ( ( $file = readdir($dh) ) !== false&#xA0; ) 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; echo &quot;filename: &quot;.$file .&quot; filetype : &quot;.filetype($dir.$file).&quot;&lt;br/&gt;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0;&#xA0; 
 }

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.opendir.php)

**[To root](/README.md)**