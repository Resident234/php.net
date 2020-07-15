# JsonSerializable::jsonSerialize



A good example on when you would use functionality like this is when working with objects.<br><br>json_encode() will take a DateTime and convert it to:<br><br>{<br>    "date":"2013-01-31 11:14:05",<br>    "timezone_type":3,<br>    "timezone":"America\/Los_Angeles"<br>}<br><br>This is great when working with PHP, but if the Date is being read by Java.  The Java date parser doesn&apos;t know what to do with that.  But it does know what to do with the ISO8601 format...<br><br>

```
<?php

date_default_timezone_set('America/Los_Angeles');

class Fruit implements JsonSerializable {
    public
        $type = 'Apple',
        $lastEaten = null;

    public function __construct() {
        $this->lastEaten = new DateTime();
    }

    public function jsonSerialize() {
        return [
            'type' => $this->type,
            'lastEaten' => $this->lastEaten->format(DateTime::ISO8601)
        ];
    }
}
echo json_encode(new Fruit()); //which outputs: {"type":"Apple","lastEaten":"2013-01-31T11:17:07-0500"}

?>
```
  

#

Nested json serializable objects will be serialized recursively. No need to call -&gt;jsonSerialize() on your own. It is especially useful in collections.<br><br>

```
<?php

class NestedSerializable implements \JsonSerializable
{

    private $serializable;

    public function __construct($serializable)
    {
        $this->serializable = $serializable;
    }

    public function jsonSerialize()
    {
        return [
            'serialized' => $this->serializable
        ];
    }

}

class SerializableCollection implements \JsonSerializable {

    private $elements;

    public function __construct(array $elements)
    {
        $this->elements = $elements;
    }

    public function jsonSerialize()
    {
        return $this->elements;
    }

}

// Outputs: [{"serialized":null},{"serialized":null},{"serialized":{"serialized":null}}]
echo json_encode(
    new SerializableCollection([
        new NestedSerializable(null),
        new NestedSerializable(null),
        new NestedSerializable(new NestedSerializable(null))
    ])
);

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/jsonserializable.jsonserialize.php)

**[To root](/README.md)**