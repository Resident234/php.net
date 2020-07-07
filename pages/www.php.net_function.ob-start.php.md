# ob_start





The following should be added: &quot;If outbut buffering is still active when the script ends, PHP outputs it automatically. In effect, every script ends with ob_end_flush().&quot;

  

#



You can use PHP to generate a static HTML page.&#xA0; Useful if you have a complex script that, for performance reasons, you do not want site visitors to run repeatedly on demand.&#xA0; A &quot;cron&quot; job can execute the PHP script to create the HTML page.&#xA0; For example:



```
<?php // CREATE index.html
&#xA0;&#xA0; ob_start();
/* PERFORM COMLEX QUERY, ECHO RESULTS, ETC. */
&#xA0;&#xA0; $page = ob_get_contents();
&#xA0;&#xA0; ob_end_clean();
&#xA0;&#xA0; $cwd = getcwd();
&#xA0;&#xA0; $file = &quot;$cwd&quot; .&apos;/&apos;. &quot;index.html&quot;;
&#xA0;&#xA0; @chmod($file,0755);
&#xA0;&#xA0; $fw = fopen($file, &quot;w&quot;);
&#xA0;&#xA0; fputs($fw,$page, strlen($page));
&#xA0;&#xA0; fclose($fw);
&#xA0;&#xA0; die();
php?>
```



  

#



If you&apos;re using object-orientated code in PHP you may, like me, want to use a call-back function that is inside an object (i.e. a class function). In this case you send ob_start a two-element array as its single argument. The first element is the name of the object (without the $ at the start), and the second is the function to call. So to use a function &apos;indent&apos; in an object called &apos;$template&apos; you would use 

```
<?php ob_start(array(&apos;template&apos;, &apos;indent&apos;)); php?>
```
.

  

#

[Official documentation page](https://www.php.net/manual/en/function.ob-start.php)

**[To root](/README.md)**