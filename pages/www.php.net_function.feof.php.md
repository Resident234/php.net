# feof





I really thought that the feof() was TRUE when the logical file pointer is a EOF.
but no ! 
we need to read and get an empty record before the eof() reports TRUE.

So

$fp = fopen(&apos;test.bin&apos;,&apos;rb&apos;);
while(!feof($fp)) {
&#xA0; $c = fgetc($fp);
&#xA0; // ... do something with $c 
&#xA0; echo ftell($fp), &quot;,&quot;;
}
echo &apos;EOF!&apos;;

prints for two time the last byte position.
If our file length is 5 byte this code prints 

0,1,2,3,4,5,5,EOF!

Because of this, you have to do another check to verify if fgetc really reads another byte (to prevent error on &quot;do something with $c&quot; ^_^).

To prevent errors you have to use this code

$fp = fopen(&apos;test.bin&apos;,&apos;rb&apos;);
while(!feof($fp)) {
&#xA0; $c = fgetc($fp);
&#xA0; if($c === false) break;
&#xA0; // ... do something with $c 
}

but this is the same of

$fp = fopen(&apos;test.bin&apos;,&apos;rb&apos;);
while(($c = fgetc($fp))!==false) {
&#xA0; // ... do something with $c 
}

Consequently feof() is simply useless.
Before write this note I want to submit this as a php bug but one php developer said that this does not imply a bug in PHP itself (http://bugs.php.net/bug.php?id=35136&amp;edit=2).

If this is not a bug I think that this need at least to be noticed.

Sorry for my bad english.
Bye ;)

  

#



When using feof() on a TCP stream, i found the following to work (after many hours of frustration and anger):

NOTE: I used &quot;;&quot; to denote the end of data transmission.&#xA0; This can be modified to whatever the server&apos;s end of file or in this case, end of output character is.



```
<?php
&#xA0; &#xA0; &#xA0; &#xA0; $cursor = &quot;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; $inData = &quot;&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; while(strcmp($cursor, &quot;;&quot;) != 0) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $cursor = fgetc($sock);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $inData.= $cursor;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; fclose($sock);
&#xA0; &#xA0; &#xA0; &#xA0; echo($inData);
?>
```


Since strcmp() returns 0 when the two strings are equal, it will return non zero as long as the cursor is not &quot;;&quot;.&#xA0; Using the above method will add &quot;;&quot; to the string, but the fix for this is simple.



```
<?php
&#xA0; &#xA0; &#xA0; &#xA0; $cursor = &quot;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; $inData = &quot;&quot;;

&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $cursor = fgetc($sock);
&#xA0; &#xA0; &#xA0; &#xA0; while(strcmp($cursor, &quot;;&quot;) != 0) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $inData.= $cursor;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; fclose($sock);
&#xA0; &#xA0; &#xA0; &#xA0; echo($inData);
?>
```


I hope this helps someone.

  

#

[Official documentation page](https://www.php.net/manual/en/function.feof.php)

**[To root](/README.md)**