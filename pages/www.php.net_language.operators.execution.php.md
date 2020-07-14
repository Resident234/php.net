# Execution Operators



Just a general usage note.  I had a very difficult time solving a problem with my script, when I accidentally put one of these backticks at the beginning of a line, like so:<br><br>[lots of code]<br>`    $URL = "blah...";<br>[more code]<br><br>Since the backtick is right above the tab key, I probably just fat-fingered it while indenting the code.<br><br>What made this so hard to find, was that PHP reported a parse error about 50 or so lines *below* the line containing the backtick.  (There were no other backticks anywhere in my code.)  And the error message was rather cryptic:<br><br>Parse error: parse error, expecting `T_STRING&apos; or `T_VARIABLE&apos; or `T_NUM_STRING&apos; in /blah.php on line 446<br><br>Just something to file away in case you&apos;re pulling your hair out trying to find an error that "isn&apos;t there."  

#

You can use variables within a pair of backticks (``).<br><br>

```
<?php
    $host = 'www.wuxiancheng.cn';
    echo `ping -n 3 {$host}`;
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.execution.php)

**[To root](/README.md)**