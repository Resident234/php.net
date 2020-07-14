# Instruction separation



Do not mis interpret<br><br>

```
<?php echo &apos;Ending tag excluded&apos;; 

with

&lt;?php echo &apos;Ending tag excluded&apos;;
&lt;p&gt;But html is still visible&lt;/p&gt;

The second one would give error. Exclude ?>
```
 if you no more html to write after the code.  

#

[Official documentation page](https://www.php.net/manual/en/language.basic-syntax.instruction-separation.php)

**[To root](/README.md)**