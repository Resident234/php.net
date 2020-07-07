# override_function





I thought the example was not very helpful, because it doesn&apos;t even override the function with another function.
My question was: If I override a function, can I call the ORIGINAL function within the OVERRIDING function?
ie, can I do this:


```
<?php
override_function(&apos;strlen&apos;, &apos;$string&apos;, &apos;return override_strlen($string);&apos;);
function override_strlen($string){
&#xA0; &#xA0; &#xA0; &#xA0; return strlen($string);&#xA0; 
}
?>
```

The answer: NO, you will get a segfault.

HOWEVER, if you use rename_function to rename the original function to a third name, then call the third name in the OVERRIDING function, you will get the desired effect:


```
<?php
rename_function(&apos;strlen&apos;, &apos;new_strlen&apos;);
override_function(&apos;strlen&apos;, &apos;$string&apos;, &apos;return override_strlen($string);&apos;);

function override_strlen($string){
&#xA0; &#xA0; &#xA0; &#xA0; return new_strlen($string);&#xA0; 
}
?>
```


I plan to use this functionality to generate log reports every time a function is called, with the parameters, time, result, etc... So to wrap a function in logging, that was what I had to do.

  

#



Overriden function name becomes __overridden__(). That&apos;s why you can&apos;t override two function, and that&apos;s how you can use the original function in the override.

  

#

[Official documentation page](https://www.php.net/manual/en/function.override-function.php)

**[To root](/README.md)**