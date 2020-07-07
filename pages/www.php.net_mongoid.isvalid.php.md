# MongoId::isValid





There is no equivalent for this method in the new extension, so instead use&#x2026;



```
<?php
if ($id instanceof \MongoDB\BSON\ObjectID
&#xA0; &#xA0; || preg_match(&apos;/^[a-f\d]{24}$/i&apos;, $id)
) {
&#xA0; &#x2026;
}
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/mongoid.isvalid.php)

**[To root](/README.md)**