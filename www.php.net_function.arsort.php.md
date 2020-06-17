# arsort




<div class="phpcode"><span class="html">
I have two servers; one running 5.6 and another that is running 7.&#xA0; Using this function on the two servers gets me different results when all of the values are the same.&#xA0; <br><br><span class="default">&lt;?php<br><br>$list </span><span class="keyword">= </span><span class="default">json_decode</span><span class="keyword">(</span><span class="string">&apos;{&quot;706&quot;:2,&quot;703&quot;:2,&quot;702&quot;:2,&quot;696&quot;:2,&quot;658&quot;:2}&apos;</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">);<br><br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$list</span><span class="keyword">);<br><br></span><span class="default">arsort</span><span class="keyword">(</span><span class="default">$list</span><span class="keyword">);<br>echo </span><span class="string">&quot;&lt;br&gt;&quot;</span><span class="keyword">;<br><br></span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$list</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>PHP 5.6 results:<br>Array ( [706] =&gt; 2 [703] =&gt; 2 [702] =&gt; 2 [696] =&gt; 2 [658] =&gt; 2 ) <br>Array ( [658] =&gt; 2 [696] =&gt; 2 [702] =&gt; 2 [703] =&gt; 2 [706] =&gt; 2 )<br><br>PHP 7 results:<br>Array ( [706] =&gt; 2 [703] =&gt; 2 [702] =&gt; 2 [696] =&gt; 2 [658] =&gt; 2 ) <br>Array ( [706] =&gt; 2 [703] =&gt; 2 [702] =&gt; 2 [696] =&gt; 2 [658] =&gt; 2 )</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you need to sort a multi-demension array, for example, an array such as 
<br>
<br>$TeamInfo[$TeamID][&quot;WinRecord&quot;] 
<br>$TeamInfo[$TeamID][&quot;LossRecord&quot;] 
<br>$TeamInfo[$TeamID][&quot;TieRecord&quot;] 
<br>$TeamInfo[$TeamID][&quot;GoalDiff&quot;]
<br>$TeamInfo[$TeamID][&quot;TeamPoints&quot;] 
<br>
<br>and you have say, 100 teams here, and want to sort by &quot;TeamPoints&quot;:
<br>
<br>first, create your multi-dimensional array. Now, create another, single dimension array populated with the scores from the first array, and with indexes of corresponding team_id... ie
<br>$foo[25] = 14
<br>$foo[47] = 42
<br>or whatever.
<br>Now, asort or arsort the second array.
<br>Since the array is now sorted by score or wins/losses or whatever you put in it, the indices are all hoopajooped.
<br>If you just walk through the array, grabbing the index of each entry, (look at the asort example. that for loop does just that) then the index you get will point right back to one of the values of the multi-dimensional array.
<br>Not sure if that&apos;s clear, but mail me if it isn&apos;t...
<br>-mo</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.arsort.php)

**[To root](/README.md)**