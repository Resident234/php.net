# Fileinfo Functions




<div class="phpcode"><span class="html">
The results of this function seem to be of dubious quality.<br><br>eg<br>1)&#xA0; a Word doc returns:<br>&apos;application/msword application/msword&apos;<br>...ok not too bad, but why does it come back twice?<br><br>2)&#xA0; a PHP file comes back as:<br>&apos;text/x-c++; charset=us-ascii&apos;<br>My test file started with &apos;&lt;?php&apos; so not ambiguous really. And where does it get the charset assumption from?<br><br>3)&#xA0; a text doc that starts with the letters &apos;GIF&apos; comes back as:<br>&apos;image/gif&apos;<br>(just like in DanielWalker&apos;s example for the unix &apos;file&apos; command)<br><br>I had better results using the PEAR &apos;MIME_Type&apos; package. It gave proper answers for 1 &amp; 3 and identified the PHP file as &apos;text/plain&apos; which is probably better than a false match for C++<br><br>Both finfo_file and MIME_Type correctly identified my other two test files which were a windows exe renamed with .doc extension, and a PDF also renamed with .doc extension.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/ref.fileinfo.php)

**[⬆ to root](/)**