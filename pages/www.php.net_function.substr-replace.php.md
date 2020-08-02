# substr_replace



It&apos;s worth noting that when start and length are both negative -and- the length is less than or equal to start, the length will have the effect of being set as 0.<br><br>

```
<?php
substr_replace('eggs','x',-1,-1); //eggxs
substr_replace('eggs','x',-1,-2); //eggxs
substr_replace('eggs','x',-1,-2); //eggxs
?>
```


Same as: 


```
<?php
substr_replace('eggs','x',-1,0); //eggxs
?>
```




```
<?php
substr_replace('huevos','x',-2,-2); //huevxos
substr_replace('huevos','x',-2,-3); //huevxos
substr_replace('huevos','x',-2,-3); //huevxos
?>
```


Same as: 


```
<?php
substr_replace('huevos','x',-2,0); //huevxos
?>
```


Another note, if length is negative and start offsets the same position as length, length (yet again) will have the effect as being set as 0. (Of course, as mentioned in the manual, when length is negative it actually represents the position before it)



```
<?php
substr_replace('abcd', 'x', 0, -4); //xabcd
?>
```


Same as: 


```
<?php
substr_replace('abcd','x',0,0); //xabcd
?>
```




```
<?php
substr_replace('abcd', 'x', 1, -3); //axbcd
?>
```


Same as:


```
<?php
substr_replace('abcd', 'x', 1, 0); //axbcd
?>
```
  

---

Forget all of the mb_substr_replace() implementations mentioned in this page, they&apos;re all buggy.<br><br>Here is a version that mimics the behavior of substr_replace() exactly:<br><br>

```
<?php

if (function_exists('mb_substr_replace') === false)
{
    function mb_substr_replace($string, $replacement, $start, $length = null, $encoding = null)
    {
        if (extension_loaded('mbstring') === true)
        {
            $string_length = (is_null($encoding) === true) ? mb_strlen($string) : mb_strlen($string, $encoding);
            
            if ($start < 0)
            {
                $start = max(0, $string_length + $start);
            }
            
            else if ($start > $string_length)
            {
                $start = $string_length;
            }
            
            if ($length < 0)
            {
                $length = max(0, $string_length - $start + $length);
            }
            
            else if ((is_null($length) === true) || ($length > $string_length))
            {
                $length = $string_length;
            }
            
            if (($start + $length) > $string_length)
            {
                $length = $string_length - $start;
            }
            
            if (is_null($encoding) === true)
            {
                return mb_substr($string, 0, $start) . $replacement . mb_substr($string, $start + $length, $string_length - $start - $length);
            }
            
            return mb_substr($string, 0, $start, $encoding) . $replacement . mb_substr($string, $start + $length, $string_length - $start - $length, $encoding);
        }
        
        return (is_null($length) === true) ? substr_replace($string, $replacement, $start) : substr_replace($string, $replacement, $start, $length);
    }
}

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.substr-replace.php)

**[To root](/README.md)**