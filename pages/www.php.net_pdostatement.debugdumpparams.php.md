# PDOStatement::debugDumpParams



This function doesn&apos;t print parameter values despite the documentation says it does. See https://bugs.php.net/bug.php?id=52384 (filed back in 2010).  

#

As noted, this doesn&#x2019;t actually simply print the prepared statement with data to be executed.<br><br>For trouble shooting purposes, I find the following useful:<br><br>

```
<?php
    function parms($string,$data) {
        $indexed=$data==array_values($data);
        foreach($data as $k=&gt;$v) {
            if(is_string($v)) $v="&apos;$v&apos;";
            if($indexed) $string=preg_replace(&apos;/\?/&apos;,$v,$string,1);
            else $string=str_replace(":$k",$v,$string);
        }
        return $string;
    }

    //    Index Parameters
        $string=&apos;INSERT INTO stuff(name,value) VALUES (?,?)&apos;;
        $data=array(&apos;Fred&apos;,23);

    //    Named Parameters
        $string=&apos;INSERT INTO stuff(name,value) VALUES (:name,:value)&apos;;
        $data=array(&apos;name&apos;=&gt;&apos;Fred&apos;,&apos;value&apos;=&gt;23);

    print parms($string,$data);
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/pdostatement.debugdumpparams.php)

**[To root](/README.md)**