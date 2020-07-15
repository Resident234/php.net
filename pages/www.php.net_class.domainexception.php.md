# The DomainException class





```
<?php
function renderImage($imageResource, $imageType)
{
  switch ($imageType) {
  case 'jpg':
  case 'jpeg':
    header('Content-type: image/jpeg');
    imagejpeg($imageResource);
    break;
  case 'png':
    header('Content-type: image/png');
    imagepng($imageResource);
    break;
  default:
    throw new DomainException('Unknown image type: ' . $imageType);
    break;
  }
  imagedestroy($imageResource);
}
?>
```
  

#

I think this kind of exception is perfect to throw when expected the  type of parameter, value etc. is good, but its value is out of domain. Look at RangeException:<br>&gt;&gt;Exception thrown to indicate range errors during program execution. Normally this means there was an arithmetic error other than under/overflow. This is the runtime version of DomainException.&lt;&lt;<br>So, this kind of exception is designed for logic error<br><br>When datatype is wrong, the better way is throwing InvalidArgumentException. <br><br>

```
<?php
// Here, use InvalidArgumentException
function media($x) {
    switch ($x) {
        case image:
            return 'PNG';
        break;
        case video:
            return 'MP4';
        break;
        default:
            throw new InvalidArgumentException ("Invalid media type!");
    }
}?>
```

This is completly diffirent situation than this:


```
<?php
// Here, use DomainException
$object = new Library ();
try {
    $object->allocate($x);
} catch (toFewMin $e) {
    throw new DomainException ("Minimal value to allocate is too high").
}
?>
```

The simillar situation, but problem occurs during runtime:


```
<?php
class library {
    function allocate($x) {
        if ($x&lt;1000)
            throw new RangeException ("Value is too low!")
    }
}
?>
```
<br>Summary: DomainException corresponds to RangeException and we should use them in simillar situations.  But first exception is designed to use when we are sure the problem is with our project, third-part elements etc. (simply: logical error), the second way is designed to use when we are sure the problem is with input data or environment (simply: runtime error).  

#



```
<?php

function divide($divident, $divisor) {
    if(!is_numeric($divident) || !is_numeric($divisor)) {
        throw new InvalidArgumentException("Function accepts only numeric values");
    }
    if($divisor == 0) {
        throw new DomainException("Divisor must not be zero");
    }
    return $divident / $divisor;
}?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.domainexception.php)

**[To root](/README.md)**