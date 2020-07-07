# mysqli_result::fetch_all





I tested using &quot;fetch all&quot; versus &quot;while / fetch array&quot; and :

fetch-all uses less memory (but not for so much).

In my case (test1 and test2): 147008,262848 bytes (fetch-all) versus 147112,262888 bytes (fetch-array &amp; while.

So, about the memory, in both cases are the same.

However, about the performance
My test takes :350ms (worst case) using fetch-all, while it takes 464ms (worst case) using fetch-array, or about 35% worst using fetch array and a while cycle.

So, using fetch-all, for a normal code that returns a moderate amount of information is :
a) cleaner (a single line of code)
b) uses less memory (about 0.01% less)
c) faster.

php 5.6 32bits, windows 8.1 64bits

  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-result.fetch-all.php)

**[To root](/README.md)**