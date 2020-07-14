# ob_start



The following should be added: "If outbut buffering is still active when the script ends, PHP outputs it automatically. In effect, every script ends with ob_end_flush()."  

#

You can use PHP to generate a static HTML page.  Useful if you have a complex script that, for performance reasons, you do not want site visitors to run repeatedly on demand.  A "cron" job can execute the PHP script to create the HTML page.  For example:<br><br>

```
<?php // CREATE index.html
   ob_start();
/* PERFORM COMLEX QUERY, ECHO RESULTS, ETC. */
   $page = ob_get_contents();
   ob_end_clean();
   $cwd = getcwd();
   $file = "$cwd" .&apos;/&apos;. "index.html";
   @chmod($file,0755);
   $fw = fopen($file, "w");
   fputs($fw,$page, strlen($page));
   fclose($fw);
   die();
?>
```
  

#

If you&apos;re using object-orientated code in PHP you may, like me, want to use a call-back function that is inside an object (i.e. a class function). In this case you send ob_start a two-element array as its single argument. The first element is the name of the object (without the $ at the start), and the second is the function to call. So to use a function &apos;indent&apos; in an object called &apos;$template&apos; you would use 

```
<?php ob_start(array(&apos;template&apos;, &apos;indent&apos;)); ?>
```
.  

#

[Official documentation page](https://www.php.net/manual/en/function.ob-start.php)

**[To root](/README.md)**