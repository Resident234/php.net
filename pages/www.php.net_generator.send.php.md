# Generator::send



Reading the example, it is a bit difficult to understand what exactly to do with this. The example below is a simple example of what you can do this.<br><br>

```
<?php
function nums() {
    for ($i = 0; $i < 5; ++$i) {
                //get a value from the caller
        $cmd = (yield $i);
        
        if($cmd == 'stop')
            return;//exit the function
        }     
}

$gen = nums();
foreach($gen as $v)
{
    if($v == 3)//we are satisfied
        $gen->send('stop');
    
    echo "{$v}\n";
}

//Output
0
1
2
3
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/generator.send.php)

**[To root](/README.md)**