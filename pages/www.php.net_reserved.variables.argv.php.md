# $argv



Please note that, $argv and $argc need to be declared global, while trying to access within a class method. <br><br>

```
<?php
class A
{
    public static function b()
    {
        var_dump($argv);
        var_dump(isset($argv));
    }
}

A::b();
?>
```
<br><br>will output NULL bool(false)  with a notice of "Undefined variable ..."<br><br>whereas global $argv fixes that.  

#

To use $_GET so you dont need to support both if it could be used from command line and from web browser.<br><br>foreach ($argv as $arg) {<br>    $e=explode("=",$arg);<br>    if(count($e)==2)<br>        $_GET[$e[0]]=$e[1];<br>    else    <br>        $_GET[$e[0]]=0;<br>}  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.argv.php)

**[To root](/README.md)**