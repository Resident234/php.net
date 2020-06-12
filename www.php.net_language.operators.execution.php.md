# Execution Operators




<div class="phpcode"><span class="html">
Just a general usage note.&#xA0; I had a very difficult time solving a problem with my script, when I accidentally put one of these backticks at the beginning of a line, like so:<br><br>[lots of code]<br>`&#xA0; &#xA0; $URL = &quot;blah...&quot;;<br>[more code]<br><br>Since the backtick is right above the tab key, I probably just fat-fingered it while indenting the code.<br><br>What made this so hard to find, was that PHP reported a parse error about 50 or so lines *below* the line containing the backtick.&#xA0; (There were no other backticks anywhere in my code.)&#xA0; And the error message was rather cryptic:<br><br>Parse error: parse error, expecting `T_STRING&apos; or `T_VARIABLE&apos; or `T_NUM_STRING&apos; in /blah.php on line 446<br><br>Just something to file away in case you&apos;re pulling your hair out trying to find an error that &quot;isn&apos;t there.&quot;</span>
</div>
  

#


<div class="phpcode"><span class="html">
You can use variables within a pair of backticks (``).<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; $host </span><span class="keyword">= </span><span class="string">&apos;www.wuxiancheng.cn&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; echo `</span><span class="string">ping -n 3 </span><span class="keyword">{</span><span class="default">$host</span><span class="keyword">}`;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.execution.php)

**[To root](/README.md)**