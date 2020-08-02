# Instruction separation



Do not mis interpret<br><br>

```
<?php echo 'Ending tag excluded'; 

with



```
<?phpphp echo 'Ending tag excluded';
<p>But html is still visible</p>

The second one would give error. Exclude ?>
```
 if you no more html to write after the code.  

---

[Official documentation page](https://www.php.net/manual/en/language.basic-syntax.instruction-separation.php)

**[To root](/README.md)**