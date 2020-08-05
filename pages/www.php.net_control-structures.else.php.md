# else



An alternative and very useful syntax is the following one:<br><br>statement ? execute if true : execute if false<br><br>Ths is very usefull for dynamic outout inside strings, for example:<br><br>print(&apos;$a is &apos; . ($a &gt; $b ? &apos;bigger than&apos; : ($a == $b ? &apos;equal to&apos; : &apos;smaler than&apos; )) .  &apos;  $b&apos;);<br><br>This will print "$a is smaler than $b" is $b is bigger than $a, "$a is bigger than $b" if $a si bigger and "$a is equal to $b" if they are same.  

---

[Official documentation page](https://www.php.net/manual/en/control-structures.else.php)

**[To root](/README.md)**