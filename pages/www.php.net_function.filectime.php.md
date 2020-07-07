# filectime





This method gets all the files in a directory, and echoes them in the order of the date they were added (by ftp or whatever).

&lt;?PHP
function dirList ($directory, $sortOrder){

&#xA0; &#xA0; //Get each file and add its details to two arrays
&#xA0; &#xA0; $results = array();
&#xA0; &#xA0; $handler = opendir($directory);
&#xA0; &#xA0; while ($file = readdir($handler)) {&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; if ($file != &apos;.&apos; &amp;&amp; $file != &apos;..&apos; &amp;&amp; $file != &quot;robots.txt&quot; &amp;&amp; $file != &quot;.htaccess&quot;){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $currentModified = filectime($directory.&quot;/&quot;.$file);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $file_names[] = $file;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $file_dates[] = $currentModified;
&#xA0; &#xA0; &#xA0; &#xA0; }&#xA0; &#xA0; 
&#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0;&#xA0; closedir($handler);

&#xA0; &#xA0; //Sort the date array by preferred order
&#xA0; &#xA0; if ($sortOrder == &quot;newestFirst&quot;){
&#xA0; &#xA0; &#xA0; &#xA0; arsort($file_dates);
&#xA0; &#xA0; }else{
&#xA0; &#xA0; &#xA0; &#xA0; asort($file_dates);
&#xA0; &#xA0; }
&#xA0; &#xA0; 
&#xA0; &#xA0; //Match file_names array to file_dates array
&#xA0; &#xA0; $file_names_Array = array_keys($file_dates);
&#xA0; &#xA0; foreach ($file_names_Array as $idx =&gt; $name) $name=$file_names[$name];
&#xA0; &#xA0; $file_dates = array_merge($file_dates);
&#xA0; &#xA0; 
&#xA0; &#xA0; $i = 0;

&#xA0; &#xA0; //Loop through dates array and then echo the list
&#xA0; &#xA0; foreach ($file_dates as $file_dates){
&#xA0; &#xA0; &#xA0; &#xA0; $date = $file_dates;
&#xA0; &#xA0; &#xA0; &#xA0; $j = $file_names_Array[$i];
&#xA0; &#xA0; &#xA0; &#xA0; $file = $file_names[$j];
&#xA0; &#xA0; &#xA0; &#xA0; $i++;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; echo&#xA0; &quot;File name: $file - Date Added: $date. &lt;br/&gt;&quot;&quot;;&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; }

}
?>
```


I hope this is useful to somebody.


  

#

[Official documentation page](https://www.php.net/manual/en/function.filectime.php)

**[To root](/README.md)**