# __halt_compiler



This function can be used in eval() -- it will halt the eval, but not the script eval"() was called in.  

#

__halt_compiler is also useful for debugging. If you need to temporarily make a change that will introduce an error later on, use __halt_compiler to prevent syntax errors. For example:<br><br>

```
<?php
if ( $something ):
  print &apos;something&apos;;
endif;   // endif placed here for debugging purposes
__halt_compiler();
endif;   // original location of endif -- would produce syntax error if __halt_compiler was not there
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.halt-compiler.php)

**[To root](/README.md)**