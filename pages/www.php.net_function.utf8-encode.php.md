# utf8_encode



Please note that utf8_encode only converts a string encoded in ISO-8859-1 to UTF-8. A more appropriate name for it would be "iso88591_to_utf8". If your text is not encoded in  ISO-8859-1, you do not need this function. If your text is already in UTF-8, you do not need this function. In fact, applying this function to text that is not encoded in ISO-8859-1 will most likely simply garble that text.<br><br>If you need to convert text from any encoding to any other encoding, look at iconv() instead.  

#

Walk through nested arrays/objects and utf8 encode all strings.<br><br>

```
<?php
// Usage
class Foo {
    public $somevar = 'whoop whoop';
}

$structure = array(
    'object' => (object) array(
        'entry' => 'hello w&#xF6;rld',
        'another_array' => array(
            'string',
            1234,
            'another string'
        )
    ),
    'string' => 'foo',
    'foo_object' => new Foo
);

utf8_encode_deep($structure);

// $structure is now utf8 encoded
print_r($structure);

// The function
function utf8_encode_deep(&amp;$input) {
    if (is_string($input)) {
        $input = utf8_encode($input);
    } else if (is_array($input)) {
        foreach ($input as &amp;$value) {
            utf8_encode_deep($value);
        }

        unset($value);
    } else if (is_object($input)) {
        $vars = array_keys(get_object_vars($input));

        foreach ($vars as $var) {
            utf8_encode_deep($input->$var);
        }
    }
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.utf8-encode.php)

**[To root](/README.md)**