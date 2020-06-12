# else




<div class="phpcode"><span class="html">
An alternative and very useful syntax is the following one:<br><br>statement ? execute if true : execute if false<br><br>Ths is very usefull for dynamic outout inside strings, for example:<br><br>print(&apos;$a is &apos; . ($a &gt; $b ? &apos;bigger than&apos; : ($a == $b ? &apos;equal to&apos; : &apos;smaler than&apos; )) .&#xA0; &apos;&#xA0; $b&apos;);<br><br>This will print &quot;$a is smaler than $b&quot; is $b is bigger than $a, &quot;$a is bigger than $b&quot; if $a si bigger and &quot;$a is equal to $b&quot; if they are same.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.else.php)

**[To root](/)**