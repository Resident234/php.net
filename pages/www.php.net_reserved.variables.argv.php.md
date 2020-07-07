# $argv





Please note that, $argv and $argc need to be declared global, while trying to access within a class method. 



```
<?php
class A
{
&#xA0; &#xA0; public static function b()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; var_dump($argv);
&#xA0; &#xA0; &#xA0; &#xA0; var_dump(isset($argv));
&#xA0; &#xA0; }
}

A::b();
?>
```


will output NULL bool(false)&#xA0; with a notice of &quot;Undefined variable ...&quot;

whereas global $argv fixes that.

  

#



To use $_GET so you dont need to support both if it could be used from command line and from web browser.

foreach ($argv as $arg) {
&#xA0; &#xA0; $e=explode(&quot;=&quot;,$arg);
&#xA0; &#xA0; if(count($e)==2)
&#xA0; &#xA0; &#xA0; &#xA0; $_GET[$e[0]]=$e[1];
&#xA0; &#xA0; else&#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; $_GET[$e[0]]=0;
}

  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.argv.php)

**[To root](/README.md)**