# fgets



A better example, to illustrate the differences in speed for large files, between fgets and stream_get_line.<br><br>This example simulates situations where you are reading potentially very long lines, of an uncertain length (but with a maximum buffer size), from an input source.<br><br>As Dade pointed out, the previous example I provided was much to easy to pick apart, and did not adequately highlight the issue I was trying to address.<br><br>Note that specifying a definitive end-character for fgets (ie: newline), generally decreases the speed difference reasonably significantly. <br><br>#!/usr/bin/php<br>

```
<?php
        $plaintext=file_get_contents('http://loripsum.net/api/60/verylong/plaintext');  # Should be around 90k characters
        $plaintext=str_replace("\n"," ",$plaintext); # Get rid of newlines

        $fp=fopen("/tmp/SourceFile.txt","w");
        for($i=0;$i&lt;100000;$i++) {
                fputs($fp,substr($plaintext,0,rand(4096,65534)) . "\n");
        }
        fclose($fp);

        $fp=fopen("/tmp/SourceFile.txt","r");
        $start=microtime(true);
        while($line=fgets($fp,65535)) {
                1;
        }
        $end=microtime(true);
        fclose($fp);
        $delta1=($end - $start);

        $fp=fopen("/tmp/SourceFile.txt","r");
        $start=microtime(true);
        while($line=stream_get_line($fp,65535)) {
                1;
        }
        $end=microtime(true);
        fclose($fp);
        $delta2=($end - $start);

        $pdiff=$delta1/$delta2;
        print "stream_get_line is " . ($pdiff&gt;1?"faster":"slower") . " than fgets - pdiff is $pdiff\n";
?>
```
<br><br>$ ./testcase.php <br>stream_get_line is faster than fgets - pdiff is 1.760398041785<br><br>Note that, in a vast majority of situations in which php is employed, tiny differences in speed between system calls are of negligible importance.  

#

[Official documentation page](https://www.php.net/manual/en/function.fgets.php)

**[To root](/README.md)**