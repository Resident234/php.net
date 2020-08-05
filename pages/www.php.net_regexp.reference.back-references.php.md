# Back references



Something similar opportunity is DEFINE.<br><br>Example:<br>    (?(DEFINE)(?&lt;myname&gt;\bvery\b))(?&amp;myname)\p{Pd}(?&amp;myname).<br><br>Expression above will match "very-very" from next sentence:<br>    Define is very-very handy sometimes.<br>              ^-------^<br><br>How it works. (?(DEFINE)(?&lt;myname&gt;\bvery\b)) - this block defines "myname" equal to "\bvery\b". So, this block "(?&amp;myname)\p{Pd}(?&amp;myname)" equvivalent to "\bvery\b\p{Pd}\bvery\b".  

---

[Official documentation page](https://www.php.net/manual/en/regexp.reference.back-references.php)

**[To root](/README.md)**