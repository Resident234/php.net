# fgets





A better example, to illustrate the differences in speed for large files, between fgets and stream_get_line.

This example simulates situations where you are reading potentially very long lines, of an uncertain length (but with a maximum buffer size), from an input source.

As Dade pointed out, the previous example I provided was much to easy to pick apart, and did not adequately highlight the issue I was trying to address.

Note that specifying a definitive end-character for fgets (ie: newline), generally decreases the speed difference reasonably significantly. 

#!/usr/bin/php


```
<?php
&#xA0; &#xA0; &#xA0; &#xA0; $plaintext=file_get_contents(&apos;http://loripsum.net/api/60/verylong/plaintext&apos;);&#xA0; # Should be around 90k characters
&#xA0; &#xA0; &#xA0; &#xA0; $plaintext=str_replace(&quot;\n&quot;,&quot; &quot;,$plaintext); # Get rid of newlines

&#xA0; &#xA0; &#xA0; &#xA0; $fp=fopen(&quot;/tmp/SourceFile.txt&quot;,&quot;w&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; for($i=0;$i&lt;100000;$i++) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; fputs($fp,substr($plaintext,0,rand(4096,65534)) . &quot;\n&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; fclose($fp);

&#xA0; &#xA0; &#xA0; &#xA0; $fp=fopen(&quot;/tmp/SourceFile.txt&quot;,&quot;r&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; $start=microtime(true);
&#xA0; &#xA0; &#xA0; &#xA0; while($line=fgets($fp,65535)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 1;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; $end=microtime(true);
&#xA0; &#xA0; &#xA0; &#xA0; fclose($fp);
&#xA0; &#xA0; &#xA0; &#xA0; $delta1=($end - $start);

&#xA0; &#xA0; &#xA0; &#xA0; $fp=fopen(&quot;/tmp/SourceFile.txt&quot;,&quot;r&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; $start=microtime(true);
&#xA0; &#xA0; &#xA0; &#xA0; while($line=stream_get_line($fp,65535)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 1;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; $end=microtime(true);
&#xA0; &#xA0; &#xA0; &#xA0; fclose($fp);
&#xA0; &#xA0; &#xA0; &#xA0; $delta2=($end - $start);

&#xA0; &#xA0; &#xA0; &#xA0; $pdiff=$delta1/$delta2;
&#xA0; &#xA0; &#xA0; &#xA0; print &quot;stream_get_line is &quot; . ($pdiff&gt;1?&quot;faster&quot;:&quot;slower&quot;) . &quot; than fgets - pdiff is $pdiff\n&quot;;
?>
```


$ ./testcase.php 
stream_get_line is faster than fgets - pdiff is 1.760398041785

Note that, in a vast majority of situations in which php is employed, tiny differences in speed between system calls are of negligible importance.

  

#

[Official documentation page](https://www.php.net/manual/en/function.fgets.php)

**[To root](/README.md)**