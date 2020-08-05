# str_getcsv



[Editor&apos;s Note (cmb): that does not produce the desired results, if fields contain linebreaks.]<br><br>Handy one liner to parse a CSV file into an array<br><br>

```
<?php

$csv = array_map('str_getcsv', file('data.csv'));

?>
```
  

---

Based on James&apos; line, this will create an array of associative arrays with the first row column headers as the keys.<br><br>

```
<?php
    $csv = array_map('str_getcsv', file($file));
    array_walk($csv, function(&amp;$a) use ($csv) {
      $a = array_combine($csv[0], $a);
    });
    array_shift($csv); # remove column header
?>
```
 <br><br>This will yield something like<br>    [2] =&gt; Array<br>        (<br>            [Campaign ID] =&gt; 295095038<br>            [Ad group ID] =&gt; 22460178158<br>            [Keyword ID] =&gt; 3993587178  

---

As the str_getcsv(), unlike to fgetcsv(), does not parse the rows in CSV string, I have found following easy workaround:<br><br>

```
<?php
$Data = str_getcsv($CsvString, "\n"); //parse the rows
foreach($Data as &amp;$Row) $Row = str_getcsv($Row, ";"); //parse the items in rows
?>
```
<br><br>Why not use explode() instead of str_getcsv() to parse rows? Because explode() would not treat possible enclosured parts of string or escaped characters correctly.  

---

Here is a quick and easy way to convert a CSV file to an associated array:<br><br>

```
<?php
/**
 * @link http://gist.github.com/385876
 */
function csv_to_array($filename='', $delimiter=',')
{
    if(!file_exists($filename) || !is_readable($filename))
        return FALSE;

    $header = NULL;
    $data = array();
    if (($handle = fopen($filename, 'r')) !== FALSE)
    {
        while (($row = fgetcsv($handle, 1000, $delimiter)) !== FALSE)
        {
            if(!$header)
                $header = $row;
            else
                $data[] = array_combine($header, $row);
        }
        fclose($handle);
    }
    return $data;
}

?>
```
  

---

Like some other users here noted, str_getcsv() cannot be used if you want to comply with either the RFC or with most spreadsheet tools like Excel or Google Docs.<br><br>These tools do not escape commas or new lines, but instead place double-quotes (") around the field. If there are any double-quotes in the field, these are escaped with another double-quote (" becomes ""). All this may look odd, but it is what the RFC and most tools do ... <br><br>For instance, try exporting as .csv a Google Docs spreadsheet (File &gt; Download as &gt; .csv) which has new lines and commas as part of the field values and see how the .csv content looks, then try to parse it using str_getcsv() ... it will spectacularly regardless of the arguments you pass to it.<br><br>Here is a function that can handle everything correctly, and more:<br><br>- doesn&apos;t use any for or while loops,<br>- it allows for any separator (any string of any length),<br>- option to skip empty lines,<br>- option to trim fields,<br>- can handle UTF8 data too (although .csv files are likely non-unicode).<br><br>Here is the more human readable version of the function:<br><br>

```
<?php

// returns a two-dimensional array or rows and fields

function parse_csv ($csv_string, $delimiter = ",", $skip_empty_lines = true, $trim_fields = true)
{
    $enc = preg_replace('/(?<!")""/', '!!Q!!', $csv_string);
    $enc = preg_replace_callback(
        '/"(.*?)"/s',
        function ($field) {
            return urlencode(utf8_encode($field[1]));
        },
        $enc
    );
    $lines = preg_split($skip_empty_lines ? ($trim_fields ? '/( *\R)+/s' : '/\R+/s') : '/\R/s', $enc);
    return array_map(
        function ($line) use ($delimiter, $trim_fields) {
            $fields = $trim_fields ? array_map('trim', explode($delimiter, $line)) : explode($delimiter, $line);
            return array_map(
                function ($field) {
                    return str_replace('!!Q!!', '"', utf8_decode(urldecode($field)));
                },
                $fields
            );
        },
        $lines
    );
}

?>
```


Since this is not using any loops, you can actually write it as a one-line statement (one-liner).

Here's the function using just one line of code for the function body, formatted nicely though:



```
<?php

// returns the same two-dimensional array as above, but with a one-liner code

function parse_csv ($csv_string, $delimiter = ",", $skip_empty_lines = true, $trim_fields = true)
{
    return array_map(
        function ($line) use ($delimiter, $trim_fields) {
            return array_map(
                function ($field) {
                    return str_replace('!!Q!!', '"', utf8_decode(urldecode($field)));
                },
                $trim_fields ? array_map('trim', explode($delimiter, $line)) : explode($delimiter, $line)
            );
        },
        preg_split(
            $skip_empty_lines ? ($trim_fields ? '/( *\R)+/s' : '/\R+/s') : '/\R/s',
            preg_replace_callback(
                '/"(.*?)"/s',
                function ($field) {
                    return urlencode(utf8_encode($field[1]));
                },
                $enc = preg_replace('/(?<!")""/', '!!Q!!', $csv_string)
            )
        )
    );
}

?>
```
<br><br>Replace !!Q!! with another placeholder if you wish.<br><br>Have fun.  

---

I wanted the best of the 2 solutions by james at moss dot io and Jay Williams (csv_to_array()) - create associative array from a CSV file with a header row.<br><br>

```
<?php

$array = array_map('str_getcsv', file('data.csv'));

$header = array_shift($array);

array_walk($array, '_combine_array', $header);

function _combine_array(&amp;$row, $key, $header) {
  $row = array_combine($header, $row);
}

?>
```
<br><br>Then I thought why not try some benchmarking? I grabbed a sample CSV file with 50,000 rows (10 columns each) and Vulcan Logic Disassembler (VLD) which hooks into the Zend Engine and dumps all the opcodes (execution units) of a script - see http://pecl.php.net/package/vld and example here: http://fabien.potencier.org/article/8/print-vs-echo-which-one-is-faster<br><br>Result: <br><br>array_walk() and array_map() - 39 opcodes<br>csv_to_array() - 69 opcodes  

---

@normadize - that is a nice start, but it fails on situations where a field is empty but quoted (returning a string with one double quote instead of an empty string) and cases like """""foo""""" that should result in ""foo"" but instead return "foo". I also get a row with 1 empty field at the end because of the final CRLF in the CSV. Plus, I don&apos;t really like the !!Q!! magic or urlencoding to get around things. Also, \R doesn&apos;t work in pcre on any of my php installations.<br><br>Here is my take on this, without anonymous functions (so it works on PHP &lt; 5.3), and without your options (because I believe the only correct way to parse according to the RFC would be $skip_empty_lines = false and $trim_fields = false).<br><br>//parse a CSV file into a two-dimensional array<br>//this seems as simple as splitting a string by lines and commas, but this only works if tricks are performed<br>//to ensure that you do NOT split on lines and commas that are inside of double quotes.<br>function parse_csv($str)<br>{<br>    //match all the non-quoted text and one series of quoted text (or the end of the string)<br>    //each group of matches will be parsed with the callback, with $matches[1] containing all the non-quoted text,<br>    //and $matches[3] containing everything inside the quotes<br>    $str = preg_replace_callback(&apos;/([^"]*)("((""|[^"])*)"|$)/s&apos;, &apos;parse_csv_quotes&apos;, $str);<br><br>    //remove the very last newline to prevent a 0-field array for the last line<br>    $str = preg_replace(&apos;/\n$/&apos;, &apos;&apos;, $str);<br><br>    //split on LF and parse each line with a callback<br>    return array_map(&apos;parse_csv_line&apos;, explode("\n", $str));<br>}<br><br>//replace all the csv-special characters inside double quotes with markers using an escape sequence<br>function parse_csv_quotes($matches)<br>{<br>    //anything inside the quotes that might be used to split the string into lines and fields later,<br>    //needs to be quoted. The only character we can guarantee as safe to use, because it will never appear in the unquoted text, is a CR<br>    //So we&apos;re going to use CR as a marker to make escape sequences for CR, LF, Quotes, and Commas.<br>    $str = str_replace("\r", "\rR", $matches[3]);<br>    $str = str_replace("\n", "\rN", $str);<br>    $str = str_replace(&apos;""&apos;, "\rQ", $str);<br>    $str = str_replace(&apos;,&apos;, "\rC", $str);<br><br>    //The unquoted text is where commas and newlines are allowed, and where the splits will happen<br>    //We&apos;re going to remove all CRs from the unquoted text, by normalizing all line endings to just LF<br>    //This ensures us that the only place CR is used, is as the escape sequences for quoted text<br>    return preg_replace(&apos;/\r\n?/&apos;, "\n", $matches[1]) . $str;<br>}<br><br>//split on comma and parse each field with a callback<br>function parse_csv_line($line)<br>{<br>    return array_map(&apos;parse_csv_field&apos;, explode(&apos;,&apos;, $line));<br>}<br><br>//restore any csv-special characters that are part of the data<br>function parse_csv_field($field) {<br>    $field = str_replace("\rC", &apos;,&apos;, $field);<br>    $field = str_replace("\rQ", &apos;"&apos;, $field);<br>    $field = str_replace("\rN", "\n", $field);<br>    $field = str_replace("\rR", "\r", $field);<br>    return $field;<br>}  

---

[Official documentation page](https://www.php.net/manual/en/function.str-getcsv.php)

**[To root](/README.md)**