# strrchr



To extract your portion of a string without the actual character you searched for, you can use:<br><br>

```
<?php
$path = &apos;/www/public_html/index.html&apos;;
$filename = substr(strrchr($path, "/"), 1);
echo $filename; // "index.html"
?>
```
  

#



```
<?php
/**
 * Removes the preceeding or proceeding portion of a string
 * relative to the last occurrence of the specified character.
 * The character selected may be retained or discarded.
 * 
 * Example usage:
 * &lt;code&gt;
 * $example = &apos;http://example.com/path/file.php&apos;;
 * $cwd_relative[] = cut_string_using_last(&apos;/&apos;, $example, &apos;left&apos;, true);
 * $cwd_relative[] = cut_string_using_last(&apos;/&apos;, $example, &apos;left&apos;, false);
 * $cwd_relative[] = cut_string_using_last(&apos;/&apos;, $example, &apos;right&apos;, true);
 * $cwd_relative[] = cut_string_using_last(&apos;/&apos;, $example, &apos;right&apos;, false);
 * foreach($cwd_relative as $string) {
 *     echo "$string &lt;br&gt;".PHP_EOL;
 * }
 * &lt;/code&gt;
 * 
 * Outputs:
 * &lt;code&gt;
 * http://example.com/path/
 * http://example.com/path
 * /file.php
 * file.php
 * &lt;/code&gt;
 * 
 * @param string $character the character to search for.
 * @param string $string the string to search through.
 * @param string $side determines whether text to the left or the right of the character is returned.
 * Options are: left, or right.
 * @param bool $keep_character determines whether or not to keep the character.
 * Options are: true, or false.
 * @return string
 */
function cut_string_using_last($character, $string, $side, $keep_character=true) {
    $offset = ($keep_character ? 1 : 0);
    $whole_length = strlen($string);
    $right_length = (strlen(strrchr($string, $character)) - 1);
    $left_length = ($whole_length - $right_length - 1);
    switch($side) {
        case &apos;left&apos;:
            $piece = substr($string, 0, ($left_length + $offset));
            break;
        case &apos;right&apos;:
            $start = (0 - ($right_length + $offset));
            $piece = substr($string, $start);
            break;
        default:
            $piece = false;
            break;
    }
    return($piece);
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.strrchr.php)

**[To root](/README.md)**