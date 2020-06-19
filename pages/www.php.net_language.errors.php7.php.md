# Errors in PHP 7




<div class="phpcode"><span class="html">
You can catch both exceptions and errors by catching(Throwable)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Throwable does not work on PHP 5.x.<br><br>To catch both exceptions and errors in PHP 5.x and 7, add a catch block for Exception AFTER catching Throwable first.<br>Once PHP 5.x support is no longer needed, the block catching Exception can be removed.<br><br>try<br>{<br>&#xA0;&#xA0; // Code that may throw an Exception or Error.<br>}<br>catch (Throwable $t)<br>{<br>&#xA0;&#xA0; // Executed only in PHP 7, will not match in PHP 5<br>}<br>catch (Exception $e)<br>{<br>&#xA0;&#xA0; // Executed only in PHP 5, will not be reached in PHP 7<br>}</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.errors.php7.php)

**[To root](/README.md)**