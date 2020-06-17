# error_reporting




<div class="phpcode"><span class="html">
If you just see a blank page instead of an error reporting and you have no server access so you can&apos;t edit php configuration files like php.ini try this:<br><br>- create a new file in which you include the faulty script:<br><br><span class="default">&lt;?php<br> error_reporting</span><span class="keyword">(</span><span class="default">E_ALL</span><span class="keyword">);<br> </span><span class="default">ini_set</span><span class="keyword">(</span><span class="string">&quot;display_errors&quot;</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);<br> include(</span><span class="string">&quot;file_with_errors.php&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>- execute this file instead of the faulty script file <br><br>now errors of your faulty script should be reported.<br>this works fine with me. hope it solves your problem as well!</span>
</div>
  

#


<div class="phpcode"><span class="html">
The example of E_ALL ^ E_NOTICE is a &apos;bit&apos; confusing for those of us not wholly conversant with bitwise operators.<br><br>If you wish to remove notices from the current level, whatever that unknown level might be, use &amp; ~ instead:<br><br><span class="default">&lt;?php<br></span><span class="comment">//....<br></span><span class="default">$errorlevel</span><span class="keyword">=</span><span class="default">error_reporting</span><span class="keyword">();<br></span><span class="default">error_reporting</span><span class="keyword">(</span><span class="default">$errorlevel </span><span class="keyword">&amp; ~</span><span class="default">E_NOTICE</span><span class="keyword">);<br></span><span class="comment">//...code that generates notices<br></span><span class="default">error_reporting</span><span class="keyword">(</span><span class="default">$errorlevel</span><span class="keyword">);<br></span><span class="comment">//...<br></span><span class="default">?&gt;<br></span><br>^ is the xor (bit flipping) operator and would actually turn notices *on* if they were previously off (in the error level on its left). It works in the example because E_ALL is guaranteed to have the bit for E_NOTICE set, so when ^ flips that bit, it is in fact turned off. &amp; ~ (and not) will always turn off the bits specified by the right-hand parameter, whether or not they were on or off.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Some E_STRICT errors seem to be thrown during the page&apos;s compilation process.&#xA0; This means they cannot be disabled by dynamically altering the error level at run time within that page.<br><br>The work-around for this was to rename the file and replace the original with a error_reporting() call and then a require() call.<br><br>Ex, rename index.php to index.inc.php, then re-create index.php as:<br><br><span class="default">&lt;?php<br>error_reporting</span><span class="keyword">(</span><span class="default">E_ALL </span><span class="keyword">&amp; ~(</span><span class="default">E_STRICT</span><span class="keyword">|</span><span class="default">E_NOTICE</span><span class="keyword">));<br>require(</span><span class="string">&apos;index.inc.php&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>That allows you to alter the error reporting prior to the file being compiled.<br><br>I discovered this recently when I was given code from another development firm that triggered several E_STRICT errors and I wanted to disable E_STRICT on a per-page basis.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.error-reporting.php)

**[To root](/README.md)**