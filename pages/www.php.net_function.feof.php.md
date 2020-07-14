# feof



I really thought that the feof() was TRUE when the logical file pointer is a EOF.<br>but no ! <br>we need to read and get an empty record before the eof() reports TRUE.<br><br>So<br><br>$fp = fopen(&apos;test.bin&apos;,&apos;rb&apos;);<br>while(!feof($fp)) {<br>  $c = fgetc($fp);<br>  // ... do something with $c <br>  echo ftell($fp), ",";<br>}<br>echo &apos;EOF!&apos;;<br><br>prints for two time the last byte position.<br>If our file length is 5 byte this code prints <br><br>0,1,2,3,4,5,5,EOF!<br><br>Because of this, you have to do another check to verify if fgetc really reads another byte (to prevent error on "do something with $c" ^_^).<br><br>To prevent errors you have to use this code<br><br>$fp = fopen(&apos;test.bin&apos;,&apos;rb&apos;);<br>while(!feof($fp)) {<br>  $c = fgetc($fp);<br>  if($c === false) break;<br>  // ... do something with $c <br>}<br><br>but this is the same of<br><br>$fp = fopen(&apos;test.bin&apos;,&apos;rb&apos;);<br>while(($c = fgetc($fp))!==false) {<br>  // ... do something with $c <br>}<br><br>Consequently feof() is simply useless.<br>Before write this note I want to submit this as a php bug but one php developer said that this does not imply a bug in PHP itself (http://bugs.php.net/bug.php?id=35136&amp;edit=2).<br><br>If this is not a bug I think that this need at least to be noticed.<br><br>Sorry for my bad english.<br>Bye ;)  

#

When using feof() on a TCP stream, i found the following to work (after many hours of frustration and anger):<br><br>NOTE: I used ";" to denote the end of data transmission.  This can be modified to whatever the server&apos;s end of file or in this case, end of output character is.<br><br>

```
<?php
        $cursor = "";
        $inData = "";

        while(strcmp($cursor, ";") != 0) {
            $cursor = fgetc($sock);
            $inData.= $cursor;
        }
        fclose($sock);
        echo($inData);
?>
```


Since strcmp() returns 0 when the two strings are equal, it will return non zero as long as the cursor is not ";".  Using the above method will add ";" to the string, but the fix for this is simple.



```
<?php
        $cursor = "";
        $inData = "";

         $cursor = fgetc($sock);
        while(strcmp($cursor, ";") != 0) {
            $inData.= $cursor;
        }
        fclose($sock);
        echo($inData);
?>
```
<br><br>I hope this helps someone.  

#

[Official documentation page](https://www.php.net/manual/en/function.feof.php)

**[To root](/README.md)**