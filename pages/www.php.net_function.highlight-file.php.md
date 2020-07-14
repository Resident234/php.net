# highlight_file



This is my try on linenumbers<br>

```
<?php
    public static function highlight_file_with_line_numbers($file) { 
          //Strip code and first span
        $code = substr(highlight_file($file, true), 36, -15);
        //Split lines
        $lines = explode(&apos;&lt;br /&gt;&apos;, $code);
        //Count
        $lineCount = count($lines);
        //Calc pad length
        $padLength = strlen($lineCount);
        
        //Re-Print the code and span again
        echo "&lt;code&gt;&lt;span style=\"color: #000000\"&gt;";
        
        //Loop lines
        foreach($lines as $i =&gt; $line) {
            //Create line number
            $lineNumber = str_pad($i + 1,  $padLength, &apos;0&apos;, STR_PAD_LEFT);
            //Print line
            echo sprintf(&apos;&lt;br&gt;&lt;span style="color: #999999"&gt;%s | &lt;/span&gt;%s&apos;, $lineNumber, $line);
        }
        
        //Close span
        echo "&lt;/span&gt;&lt;/code&gt;";
    }

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.highlight-file.php)

**[To root](/README.md)**