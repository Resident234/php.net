# $_GET




<div class="phpcode"><span class="html">
Just a note, because I didn&apos;t know for sure until I tested it.<br><br>If you have a query string that contains a parameter but no value (not even an equals sign), like so:<br><a href="http://path/to/script.php?a" rel="nofollow" target="_blank">http://path/to/script.php?a</a><br><br>The following script is a good test to determine how a is valued:<br>&lt;pre&gt;<br><span class="default">&lt;?php<br> print_r</span><span class="keyword">(</span><span class="default">$_GET</span><span class="keyword">);<br> if(</span><span class="default">$_GET</span><span class="keyword">[</span><span class="string">&quot;a&quot;</span><span class="keyword">] === </span><span class="string">&quot;&quot;</span><span class="keyword">) echo </span><span class="string">&quot;a is an empty string\n&quot;</span><span class="keyword">;<br> if(</span><span class="default">$_GET</span><span class="keyword">[</span><span class="string">&quot;a&quot;</span><span class="keyword">] === </span><span class="default">false</span><span class="keyword">) echo </span><span class="string">&quot;a is false\n&quot;</span><span class="keyword">;<br> if(</span><span class="default">$_GET</span><span class="keyword">[</span><span class="string">&quot;a&quot;</span><span class="keyword">] === </span><span class="default">null</span><span class="keyword">) echo </span><span class="string">&quot;a is null\n&quot;</span><span class="keyword">;<br> if(isset(</span><span class="default">$_GET</span><span class="keyword">[</span><span class="string">&quot;a&quot;</span><span class="keyword">])) echo </span><span class="string">&quot;a is set\n&quot;</span><span class="keyword">;<br> if(!empty(</span><span class="default">$_GET</span><span class="keyword">[</span><span class="string">&quot;a&quot;</span><span class="keyword">])) echo </span><span class="string">&quot;a is not empty&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span>&lt;/pre&gt;<br><br>I tested this with script.php?a, and it returned:<br><br>a is an empty string<br>a is set<br><br>So note that a parameter with no value associated with, even without an equals sign, is considered to be an empty string (&quot;&quot;), isset() returns true for it, and it is considered empty, but not false or null. Seems obvious after the first test, but I just had to make sure.<br><br>Of course, if I do not include it in my browser query, the script returns<br>Array<br>(<br>)<br>a is null</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that named anchors are not part of the query string and are never submitted by the browser to the server.<br><br>Eg.<br><a href="http://www.xyz-abc.kz/index.php?title=apocalypse.php#doom" rel="nofollow" target="_blank">http://www.xyz-abc.kz/index.php?title=apocalypse.php#doom</a><br><br>echo $_GET[&apos;title&apos;]; <br><br>// returns &quot;apocalypse.php&quot; and NOT &quot;apocalypse.php#doom&quot;<br><br>you would be better off treating the named anchor as another query string variable like so:<br><a href="http://www.xyz-abc.kz/index.php?title=apocalypse.php&amp;na=doom" rel="nofollow" target="_blank">http://www.xyz-abc.kz/index.php?title=apocalypse.php&amp;na=doom</a><br><br>...and then retrieve it using something like this:<br>$url = $_GET[&apos;title&apos;].&quot;#&quot;.$_GET[&apos;na&apos;];<br><br>Hope this helps someone...</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.get.php)

**[To root](/README.md)**