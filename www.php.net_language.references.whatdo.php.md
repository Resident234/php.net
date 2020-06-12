# What References Do




<div class="phpcode"><span class="html">
I ran into something when using an expanded version of the example of pbaltz at NO_SPAM dot cs dot NO_SPAM dot wisc dot edu below.<br>This could be somewhat confusing although it is perfectly clear if you have read the manual carfully. It makes the fact that references always point to the content of a variable perfectly clear (at least to me).<br><br><span class="default">&lt;?php<br>$a </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;<br></span><span class="default">$c </span><span class="keyword">= </span><span class="default">2</span><span class="keyword">;<br></span><span class="default">$b </span><span class="keyword">=&amp; </span><span class="default">$a</span><span class="keyword">; </span><span class="comment">// $b points to 1<br></span><span class="default">$a </span><span class="keyword">=&amp; </span><span class="default">$c</span><span class="keyword">; </span><span class="comment">// $a points now to 2, but $b still to 1;<br></span><span class="keyword">echo </span><span class="default">$a</span><span class="keyword">, </span><span class="string">&quot; &quot;</span><span class="keyword">, </span><span class="default">$b</span><span class="keyword">;<br></span><span class="comment">// Output: 2 1<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Watch out for this:<br><br>foreach ($somearray as &amp;$i) {<br>&#xA0; // update some $i...<br>}<br>...<br>foreach ($somearray as $i) {<br>&#xA0; // last element of $somearray is mysteriously overwritten!<br>}<br><br>Problem is $i contians reference to last element of $somearray after the first foreach, and the second foreach happily assigns to it!</span>
</div>
  

#


<div class="phpcode"><span class="html">
It appears that references can have side-effects.&#xA0; Below are two examples.&#xA0; Both are simply copying one array to another.&#xA0; In the second example, a reference is made to a value in the first array before the copy.&#xA0; In the first example the value at index 0 points to two separate memory locations. In the second example, the value at index 0 points to the same memory location. <br><br>I won&apos;t say this is a bug, because I don&apos;t know what the designed behavior of PHP is, but I don&apos;t think ANY developers would expect this behavior, so look out.<br><br>An example of where this could cause problems is if you do an array copy in a script and expect on type of behavior, but then later add a reference to a value in the array earlier in the script, and then find that the array copy behavior has unexpectedly changed.<br><br><span class="default">&lt;?php<br></span><span class="comment">// Example one<br></span><span class="default">$arr1 </span><span class="keyword">= array(</span><span class="default">1</span><span class="keyword">);<br>echo </span><span class="string">&quot;\nbefore:\n&quot;</span><span class="keyword">;<br>echo </span><span class="string">&quot;\$arr1[0] == </span><span class="keyword">{</span><span class="default">$arr1</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]}</span><span class="string">\n&quot;</span><span class="keyword">;<br></span><span class="default">$arr2 </span><span class="keyword">= </span><span class="default">$arr1</span><span class="keyword">;<br></span><span class="default">$arr2</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]++;<br>echo </span><span class="string">&quot;\nafter:\n&quot;</span><span class="keyword">;<br>echo </span><span class="string">&quot;\$arr1[0] == </span><span class="keyword">{</span><span class="default">$arr1</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]}</span><span class="string">\n&quot;</span><span class="keyword">;<br>echo </span><span class="string">&quot;\$arr2[0] == </span><span class="keyword">{</span><span class="default">$arr2</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]}</span><span class="string">\n&quot;</span><span class="keyword">;<br><br></span><span class="comment">// Example two<br></span><span class="default">$arr3 </span><span class="keyword">= array(</span><span class="default">1</span><span class="keyword">);<br></span><span class="default">$a </span><span class="keyword">=&amp; </span><span class="default">$arr3</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">];<br>echo </span><span class="string">&quot;\nbefore:\n&quot;</span><span class="keyword">;<br>echo </span><span class="string">&quot;\$a == </span><span class="default">$a</span><span class="string">\n&quot;</span><span class="keyword">;<br>echo </span><span class="string">&quot;\$arr3[0] == </span><span class="keyword">{</span><span class="default">$arr3</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]}</span><span class="string">\n&quot;</span><span class="keyword">;<br></span><span class="default">$arr4 </span><span class="keyword">= </span><span class="default">$arr3</span><span class="keyword">;<br></span><span class="default">$arr4</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]++;<br>echo </span><span class="string">&quot;\nafter:\n&quot;</span><span class="keyword">;<br>echo </span><span class="string">&quot;\$a == </span><span class="default">$a</span><span class="string">\n&quot;</span><span class="keyword">;<br>echo </span><span class="string">&quot;\$arr3[0] == </span><span class="keyword">{</span><span class="default">$arr3</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]}</span><span class="string">\n&quot;</span><span class="keyword">;<br>echo </span><span class="string">&quot;\$arr4[0] == </span><span class="keyword">{</span><span class="default">$arr4</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]}</span><span class="string">\n&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.references.whatdo.php)

**[⬆ to root](/)**