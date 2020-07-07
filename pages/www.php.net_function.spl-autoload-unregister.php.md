# spl_autoload_unregister




<div class="phpcode"><span class="html">
$functions = spl_autoload_functions();<br>&#xA0; &#xA0; foreach($functions as $function) {<br>&#xA0; &#xA0; &#xA0; &#xA0; spl_autoload_unregister($function);<br>&#xA0; &#xA0; }<br><br>A nice way to unregister all functions.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.spl-autoload-unregister.php)

**[To root](/README.md)**