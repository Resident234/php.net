# Fileinfo Functions



The results of this function seem to be of dubious quality.<br><br>eg<br>1)  a Word doc returns:<br>&apos;application/msword application/msword&apos;<br>...ok not too bad, but why does it come back twice?<br><br>2)  a PHP file comes back as:<br>&apos;text/x-c++; charset=us-ascii&apos;<br>My test file started with &apos;

```
<?php' so not ambiguous really. And where does it get the charset assumption from?

3)  a text doc that starts with the letters 'GIF' comes back as:
'image/gif'
(just like in DanielWalker's example for the unix 'file' command)

I had better results using the PEAR 'MIME_Type' package. It gave proper answers for 1 &amp; 3 and identified the PHP file as 'text/plain' which is probably better than a false match for C++

Both finfo_file and MIME_Type correctly identified my other two test files which were a windows exe renamed with .doc extension, and a PDF also renamed with .doc extension.?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/ref.fileinfo.php)

**[To root](/README.md)**