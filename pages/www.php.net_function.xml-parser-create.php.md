# xml_parser_create



I created a function, which combines xml_paresr_create and all functions around.<br><br>

```
<?php
function html_parse($file)
     {
      $array = str_split($file, 1);
      $count = false;
      $text = "";
      $end = false;
      foreach($array as $temp)
       {
        switch($temp)
         {
          case "<":
           between($text);
           $text = "";
           $count = true;
           $end = false;
           break;
          case ">":
           if($end == true) {end_tag($text);}
           else {start_tag($text);}
           $text = "";
           break;
          case "/":
           if($count == true) {$end = true;}
           else {$text = $text . "/";}
           break;
          default:
           $count = false;
           $text = $text . $temp;
         }
       }
     }
?>
```

The input value is a string.
It calls functions start_tag() , between() and end_tag() just like the original xml parser.

But it has a few differences:
  - It does NOT check the code. Just resends values to that three functions, no matter, if they are right
  - It works with parameters. For example: from tag <sth b="42"> sends sth b="42"
  - It works wit diacritics. The original parser sometimes wrapped the text before the first diacritics appearance.
  - Works with all encoding. If the input is UTF-8, the output will be UTF-8 too
  - It works with strings. Not with file pointers.
  - No "Reserved XML name" error
  - No doctype needed
  - It does not work with commentaries, notes, programming instructions etc. Just the tags

definition of the handling functions is:



```
<?php
function between($stuff) {}
?>
```
<br><br>No other attributes  

---

[Official documentation page](https://www.php.net/manual/en/function.xml-parser-create.php)

**[To root](/README.md)**