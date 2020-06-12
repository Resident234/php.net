# require_once




<div class="phpcode"><span class="html">
If your code is running on multiple servers with different environments (locations from where your scripts run) the following idea may be useful to you:
<br>
<br>a. Do not give absolute path to include files on your server.
<br>b. Dynamically calculate the full path (absolute path)
<br>
<br>Hints:
<br>Use a combination of dirname(__FILE__) and subsequent calls to itself until you reach to the home of your &apos;/index.php&apos;. Then, attach this variable (that contains the path) to your included files.
<br>
<br>One of my typical example is:
<br>
<br><span class="default">&lt;?php
<br>define</span><span class="keyword">(</span><span class="string">&apos;__ROOT__&apos;</span><span class="keyword">, </span><span class="default">dirname</span><span class="keyword">(</span><span class="default">dirname</span><span class="keyword">(</span><span class="default">__FILE__</span><span class="keyword">)));
<br>require_once(</span><span class="default">__ROOT__</span><span class="keyword">.</span><span class="string">&apos;/config.php&apos;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>instead of:
<br><span class="default">&lt;?php </span><span class="keyword">require_once(</span><span class="string">&apos;/var/www/public_html/config.php&apos;</span><span class="keyword">); </span><span class="default">?&gt;
<br></span>
<br>After this, if you copy paste your codes to another servers, it will still run, without requiring any further re-configurations.
<br>
<br>[EDIT BY danbrown AT php DOT net: Contains a typofix (missing &apos;)&apos;) provided by &apos;JoeB&apos; on 09-JUN-2011.]</span>
</div>
  

#


<div class="phpcode"><span class="html">
require_once may not work correctly inside repetitive function when storing variable for example:<br><br>file: var.php<br><span class="default">&lt;?php<br>$foo </span><span class="keyword">= </span><span class="string">&apos;bar&apos;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>file: check.php<br><span class="default">&lt;?php<br><br></span><span class="keyword">function </span><span class="default">foo</span><span class="keyword">(){<br>&#xA0; &#xA0; require_once(</span><span class="string">&apos;var.php&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">$foo</span><span class="keyword">;<br>}<br><br>for(</span><span class="default">$a</span><span class="keyword">=</span><span class="default">1</span><span class="keyword">;</span><span class="default">$a</span><span class="keyword">&lt;=</span><span class="default">5</span><span class="keyword">;</span><span class="default">$a</span><span class="keyword">++){<br>&#xA0; &#xA0; echo </span><span class="default">foo</span><span class="keyword">().</span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;<br>}<br><br>&gt; </span><span class="default">php check</span><span class="keyword">.</span><span class="default">php<br>result</span><span class="keyword">: <br></span><span class="default">bar<br></span><span class="keyword">&lt;empty </span><span class="default">line</span><span class="keyword">&gt;<br>&lt;empty </span><span class="default">line</span><span class="keyword">&gt;<br>&lt;empty </span><span class="default">line</span><span class="keyword">&gt;<br>&lt;empty </span><span class="default">line</span><span class="keyword">&gt;<br><br></span><span class="default">to make sure variable bar available at each </span><span class="keyword">function </span><span class="default">call</span><span class="keyword">, </span><span class="default">replace </span><span class="keyword">require </span><span class="default">once with </span><span class="keyword">require. </span><span class="default">eg situation</span><span class="keyword">: </span><span class="default">https</span><span class="keyword">:</span><span class="comment">//stackoverflow.com/questions/29898199/variables-not-defined-inside-function-on-second-time-at-foreach<br><br></span><span class="default">Solution</span><span class="keyword">:<br><br></span><span class="default">file</span><span class="keyword">: </span><span class="default">check2</span><span class="keyword">.</span><span class="default">php<br></span><span class="keyword">&lt;?</span><span class="default">php<br><br></span><span class="keyword">function </span><span class="default">foo</span><span class="keyword">(){<br>&#xA0; &#xA0; require(</span><span class="string">&apos;var.php&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">$foo</span><span class="keyword">;<br>}<br><br>for(</span><span class="default">$a</span><span class="keyword">=</span><span class="default">1</span><span class="keyword">;</span><span class="default">$a</span><span class="keyword">&lt;=</span><span class="default">5</span><span class="keyword">;</span><span class="default">$a</span><span class="keyword">++){<br>&#xA0; &#xA0; echo </span><span class="default">foo</span><span class="keyword">().</span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;<br>}<br><br>&gt; </span><span class="default">php check2</span><span class="keyword">.</span><span class="default">php<br>result</span><span class="keyword">:<br></span><span class="default">bar<br>bar<br>bar<br>bar<br>bar</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
&quot;require_once&quot; and &quot;require&quot; are language constructs and not functions. Therefore they should be written without &quot;()&quot; brackets!</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.require-once.php)

**[To root](/)**