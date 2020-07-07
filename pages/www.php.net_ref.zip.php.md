# Zip Functions





This is the function I use to unzip a file. 
It includes the following options:
* Unzip in any directory you like
* Unzip in the directory of the zip file
* Unzip in a directory with the zipfile&apos;s name in the directory of the zip file. (i.e.: C:\test.zip will be unzipped in&#xA0; C:\test\)
* Overwrite existing files or not
* It creates non existing directories with the function Create_dirs($path)

You should use absolute paths with slashes (/) instead of backslashes (\).
I tested it with PHP 5.2.0 with php_zip.dll extension loaded



```
<?php
/**
 * Unzip the source_file in the destination dir
 *
 * @param&#xA0;&#xA0; string&#xA0; &#xA0; &#xA0; The path to the ZIP-file.
 * @param&#xA0;&#xA0; string&#xA0; &#xA0; &#xA0; The path where the zipfile should be unpacked, if false the directory of the zip-file is used
 * @param&#xA0;&#xA0; boolean&#xA0; &#xA0;&#xA0; Indicates if the files will be unpacked in a directory with the name of the zip-file (true) or not (false) (only if the destination directory is set to false!)
 * @param&#xA0;&#xA0; boolean&#xA0; &#xA0;&#xA0; Overwrite existing files (true) or not (false)
 *&#xA0; 
 * @return&#xA0; boolean&#xA0; &#xA0;&#xA0; Succesful or not
 */
function unzip($src_file, $dest_dir=false, $create_zip_name_dir=true, $overwrite=true) 
{
&#xA0; if ($zip = zip_open($src_file)) 
&#xA0; {
&#xA0; &#xA0; if ($zip) 
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; $splitter = ($create_zip_name_dir === true) ? &quot;.&quot; : &quot;/&quot;;
&#xA0; &#xA0; &#xA0; if ($dest_dir === false) $dest_dir = substr($src_file, 0, strrpos($src_file, $splitter)).&quot;/&quot;;
&#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; // Create the directories to the destination dir if they don&apos;t already exist
&#xA0; &#xA0; &#xA0; create_dirs($dest_dir);

&#xA0; &#xA0; &#xA0; // For every file in the zip-packet
&#xA0; &#xA0; &#xA0; while ($zip_entry = zip_read($zip)) 
&#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; // Now we&apos;re going to create the directories in the destination directories
&#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; // If the file is not in the root dir
&#xA0; &#xA0; &#xA0; &#xA0; $pos_last_slash = strrpos(zip_entry_name($zip_entry), &quot;/&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; if ($pos_last_slash !== false)
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Create the directory where the zip-entry should be saved (with a &quot;/&quot; at the end)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; create_dirs($dest_dir.substr(zip_entry_name($zip_entry), 0, $pos_last_slash+1));
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; // Open the entry
&#xA0; &#xA0; &#xA0; &#xA0; if (zip_entry_open($zip,$zip_entry,&quot;r&quot;)) 
&#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // The name of the file to save on the disk
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $file_name = $dest_dir.zip_entry_name($zip_entry);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Check if the files should be overwritten or not
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ($overwrite === true || $overwrite === false &amp;&amp; !is_file($file_name))
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Get the content of the zip entry
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $fstream = zip_entry_read($zip_entry, zip_entry_filesize($zip_entry));

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; file_put_contents($file_name, $fstream );
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Set the rights
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; chmod($file_name, 0777);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo &quot;save: &quot;.$file_name.&quot;&lt;br /&gt;&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Close the entry
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; zip_entry_close($zip_entry);
&#xA0; &#xA0; &#xA0; &#xA0; }&#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; // Close the zip-file
&#xA0; &#xA0; &#xA0; zip_close($zip);
&#xA0; &#xA0; }
&#xA0; } 
&#xA0; else
&#xA0; {
&#xA0; &#xA0; return false;
&#xA0; }
&#xA0; 
&#xA0; return true;
}

/**
 * This function creates recursive directories if it doesn&apos;t already exist
 *
 * @param String&#xA0; The path that should be created
 *&#xA0; 
 * @return&#xA0; void
 */
function create_dirs($path)
{
&#xA0; if (!is_dir($path))
&#xA0; {
&#xA0; &#xA0; $directory_path = &quot;&quot;;
&#xA0; &#xA0; $directories = explode(&quot;/&quot;,$path);
&#xA0; &#xA0; array_pop($directories);
&#xA0; &#xA0; 
&#xA0; &#xA0; foreach($directories as $directory)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; $directory_path .= $directory.&quot;/&quot;;
&#xA0; &#xA0; &#xA0; if (!is_dir($directory_path))
&#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; mkdir($directory_path);
&#xA0; &#xA0; &#xA0; &#xA0; chmod($directory_path, 0777);
&#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }
&#xA0; }
}

// Extract C:/zipfiletest/zip-file.zip to C:/zipfiletest/zip-file/ and overwrites existing files
unzip(&quot;C:/zipfiletest/zip-file.zip&quot;, false, true, true);

// Extract C:/zipfiletest/zip-file.zip to C:/another_map/zipfiletest/ and doesn&apos;t overwrite existing files. NOTE: It doesn&apos;t create a map with the zip-file-name!
unzip(&quot;C:/zipfiletest/zip-file.zip&quot;, &quot;C:/another_map/zipfiletest/&quot;, true, false);

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/ref.zip.php)

**[To root](/README.md)**