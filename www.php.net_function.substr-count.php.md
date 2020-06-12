# substr_count




<div class="phpcode"><span class="html">
It&apos;s worth noting this function is surprisingly fast. I first ran it against a ~500KB string on our web server. It found 6 occurrences of the needle I was looking for in 0.0000 seconds. Yes, it ran faster than microtime() could measure.<br><br>Looking to give it a challenge, I then ran it on a Mac laptop from 2010 against a 120.5MB string. For one test needle, it found 2385 occurrences in 0.0266 seconds. Another test needs found 290 occurrences in 0.114 seconds.<br><br>Long story short, if you&apos;re wondering whether this function is slowing down your script, the answer is probably not.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Making this case insensitive is easy for anyone who needs this.&#xA0; Simply convert the haystack and the needle to the same case (upper or lower).<br><br>substr_count(strtoupper($haystack), strtoupper($needle))</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.substr-count.php)

**[⬆ to root](/)**