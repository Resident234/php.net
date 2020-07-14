# MongoId::isValid



There is no equivalent for this method in the new extension, so instead use&#x2026;<br><br>

```
<?php
if ($id instanceof \MongoDB\BSON\ObjectID
    || preg_match(&apos;/^[a-f\d]{24}$/i&apos;, $id)
) {
  &#x2026;
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/mongoid.isvalid.php)

**[To root](/README.md)**