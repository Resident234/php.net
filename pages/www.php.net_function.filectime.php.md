# filectime



This method gets all the files in a directory, and echoes them in the order of the date they were added (by ftp or whatever).<br><br>&lt;?PHP<br>function dirList ($directory, $sortOrder){<br><br>    //Get each file and add its details to two arrays<br>    $results = array();<br>    $handler = opendir($directory);<br>    while ($file = readdir($handler)) {  <br>        if ($file != &apos;.&apos; &amp;&amp; $file != &apos;..&apos; &amp;&amp; $file != "robots.txt" &amp;&amp; $file != ".htaccess"){<br>            $currentModified = filectime($directory."/".$file);<br>            $file_names[] = $file;<br>            $file_dates[] = $currentModified;<br>        }    <br>    }<br>       closedir($handler);<br><br>    //Sort the date array by preferred order<br>    if ($sortOrder == "newestFirst"){<br>        arsort($file_dates);<br>    }else{<br>        asort($file_dates);<br>    }<br>    <br>    //Match file_names array to file_dates array<br>    $file_names_Array = array_keys($file_dates);<br>    foreach ($file_names_Array as $idx =&gt; $name) $name=$file_names[$name];<br>    $file_dates = array_merge($file_dates);<br>    <br>    $i = 0;<br><br>    //Loop through dates array and then echo the list<br>    foreach ($file_dates as $file_dates){<br>        $date = $file_dates;<br>        $j = $file_names_Array[$i];<br>        $file = $file_names[$j];<br>        $i++;<br>            <br>        echo  "File name: $file - Date Added: $date. &lt;br/&gt;"";        <br>    }<br><br>}<br>?>
```
<br><br>I hope this is useful to somebody.  

#

[Official documentation page](https://www.php.net/manual/en/function.filectime.php)

**[To root](/README.md)**