# array_uintersect



I want to stress that in the user function, you do need to return either a 1 or a -1 properly; you cannot simply return 0 if the results are equal and 1 if they are not.  <br><br>The following code is incorrect:<br><br>

```
<?php
function myfunction($v1,$v2) 
{
if ($v1===$v2)
    {
    return 0;
    }
return 1;
}

$a1=array(1, 2, 4);
$a2=array(1, 3, 4);
print_r(array_uintersect($a1,$a2,"myfunction"));
?>
```


This code is correct:



```
<?php
function myfunction($v1,$v2) 
{
if ($v1===$v2)
    {
    return 0;
    }
if ($v1 &gt; $v2) return 1;
return -1;
}
$a1=array(1, 2, 4);
$a2=array(1, 3, 4);
print_r(array_uintersect($a1,$a2,"myfunction"));
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.array-uintersect.php)

**[To root](/README.md)**