# Execution Operators





Just a general usage note.&#xA0; I had a very difficult time solving a problem with my script, when I accidentally put one of these backticks at the beginning of a line, like so:

[lots of code]
`&#xA0; &#xA0; $URL = &quot;blah...&quot;;
[more code]

Since the backtick is right above the tab key, I probably just fat-fingered it while indenting the code.

What made this so hard to find, was that PHP reported a parse error about 50 or so lines *below* the line containing the backtick.&#xA0; (There were no other backticks anywhere in my code.)&#xA0; And the error message was rather cryptic:

Parse error: parse error, expecting `T_STRING&apos; or `T_VARIABLE&apos; or `T_NUM_STRING&apos; in /blah.php on line 446

Just something to file away in case you&apos;re pulling your hair out trying to find an error that &quot;isn&apos;t there.&quot;

  

#



You can use variables within a pair of backticks (``).



```
<?php
&#xA0; &#xA0; $host = &apos;www.wuxiancheng.cn&apos;;
&#xA0; &#xA0; echo `ping -n 3 {$host}`;
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.execution.php)

**[To root](/README.md)**