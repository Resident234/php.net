# override_function



I thought the example was not very helpful, because it doesn&apos;t even override the function with another function.<br>My question was: If I override a function, can I call the ORIGINAL function within the OVERRIDING function?<br>ie, can I do this:<br>

```
<?php
override_function('strlen', '$string', 'return override_strlen($string);');
function override_strlen($string){
        return strlen($string);  
}
?>
```

The answer: NO, you will get a segfault.

HOWEVER, if you use rename_function to rename the original function to a third name, then call the third name in the OVERRIDING function, you will get the desired effect:


```
<?php
rename_function('strlen', 'new_strlen');
override_function('strlen', '$string', 'return override_strlen($string);');

function override_strlen($string){
        return new_strlen($string);  
}
?>
```
<br><br>I plan to use this functionality to generate log reports every time a function is called, with the parameters, time, result, etc... So to wrap a function in logging, that was what I had to do.  

#

Overriden function name becomes __overridden__(). That&apos;s why you can&apos;t override two function, and that&apos;s how you can use the original function in the override.  

#

[Official documentation page](https://www.php.net/manual/en/function.override-function.php)

**[To root](/README.md)**