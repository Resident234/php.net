# PDOStatement::debugDumpParams





This function doesn&apos;t print parameter values despite the documentation says it does. See https://bugs.php.net/bug.php?id=52384 (filed back in 2010).

  

#



As noted, this doesn&#x2019;t actually simply print the prepared statement with data to be executed.

For trouble shooting purposes, I find the following useful:



```
<?php
&#xA0; &#xA0; function parms($string,$data) {
&#xA0; &#xA0; &#xA0; &#xA0; $indexed=$data==array_values($data);
&#xA0; &#xA0; &#xA0; &#xA0; foreach($data as $k=&gt;$v) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(is_string($v)) $v=&quot;&apos;$v&apos;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if($indexed) $string=preg_replace(&apos;/\?/&apos;,$v,$string,1);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else $string=str_replace(&quot;:$k&quot;,$v,$string);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; return $string;
&#xA0; &#xA0; }

&#xA0; &#xA0; //&#xA0; &#xA0; Index Parameters
&#xA0; &#xA0; &#xA0; &#xA0; $string=&apos;INSERT INTO stuff(name,value) VALUES (?,?)&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; $data=array(&apos;Fred&apos;,23);

&#xA0; &#xA0; //&#xA0; &#xA0; Named Parameters
&#xA0; &#xA0; &#xA0; &#xA0; $string=&apos;INSERT INTO stuff(name,value) VALUES (:name,:value)&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; $data=array(&apos;name&apos;=&gt;&apos;Fred&apos;,&apos;value&apos;=&gt;23);

&#xA0; &#xA0; print parms($string,$data);
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.debugdumpparams.php)

**[To root](/README.md)**