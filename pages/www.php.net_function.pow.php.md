# pow





Many notations use &quot;^&quot; as a power operator, but in PHP (and other C-based languages) that is actually the XOR operator. You need to use this &apos;pow&apos; function, there is no power operator.

i.e. 3^2 means &quot;3 XOR 2&quot; not &quot;3 squared&quot;.

It is particular confusing as when doing Pythagoras theorem in a &apos;closet points&apos; algorithm using &quot;^&quot; you get results that look vaguely correct but with an error.

  

#



Note that pow(0, 0) equals to 1 although mathematically this is undefined.

  

#

[Official documentation page](https://www.php.net/manual/en/function.pow.php)

**[To root](/README.md)**