# fgetc




<div class="phpcode"><span class="html">
The best and simplest way to get input from a user in the CLI with only PHP is to use fgetc() function with the STDIN constant:<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">echo </span><span class="string">&apos;Are you sure you want to quit? (y/n) &apos;</span><span class="keyword">;<br></span><span class="default">$input </span><span class="keyword">= </span><span class="default">fgetc</span><span class="keyword">(</span><span class="default">STDIN</span><span class="keyword">);<br><br>if (</span><span class="default">$input </span><span class="keyword">== </span><span class="string">&apos;y&apos;</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; exit(</span><span class="default">0</span><span class="keyword">);<br>}<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.fgetc.php)

**[To root](/README.md)**