# ob_clean





@cornel: It&apos;s easy enough to say &quot;Don&apos;t do that&quot; when you think you&apos;ve got the person right in front of you. But one doesn&apos;t always have the original coder, or even one of a dozen of the original coders. Are you really suggesting that it would be wrong to use this function as a band-aid when the alternative may be looking through hundreds of source files you didn&apos;t write for errors you didn&apos;t introduce?

To your point, though, it is (or should be) a commonly accepted best practice to not put closing PHP tags at the end of files. When, however, enforcing that would take a time machine, it&apos;s appropriate to use ob_clean() as a band-aid to make dynamically generated images work as expected.

  

#



Don&apos;t use ob_clean() to clear white-space or other unwanted content &quot;accidentally&quot; generated by included files.
Included files should not generate unwanted content in the first place.
If they do, You are doing something wrong, like inserting white-space after &quot;?>
```
&quot; (there should not be a &quot;?>
```
&quot; at the end of PHP files: http://php.net/manual/en/language.basic-syntax.phptags.php ).

  

#



I find this function incredibly useful when manipulating or creating images in php (with GD).

I spent quite a while searching through a large number of included files to find where I had a undesired space after php&apos;s ending tag - as this was causing all my images on the fly to break due to output already being set. Even more annoying was that this was not caught not php&apos;s error reporting so there was no reference to the problem line(s) in my log file. I don&apos;t know why error reporting wouldn&apos;t catch this since it was set to accept warnings, and the same thing had been caught in the past.

Nevertheless, I never did find the line(s) that were adding extra spaces or new lines before my images were being generated, but what I did instead was add this handy function right before my image manipulation code and right after the include/require code.

For example:



```
<?php

// require some external library files
require (&quot;lib/somelibrary.php&quot;);
require (&quot;lib/class/someclass.php&quot;);

// clean the output buffer
ob_clean();

// simple test image
header(&quot;Content-type: image/gif&quot;);
$im = imagecreate (100, 50);
imagegif($im);
imagedestroy($im);

?>
```


While this may seem trivial a trivial use of the function, it in fact is incredibly useful for insuring no extra spaces or new lines have already been output while making images in php. As many of you probably already know, extra lines, spacing and padding that appears prior to image-code will prevent the image from being created. If the file &quot;lib/somelibrary.php&quot; had so much as an extra new line after the closing php tag then it would completely prevent the image from working in the above script.

If you work on an extremely large project with a lot of source and required files, like myself, you will be well-advised to always clear the output buffer prior to creating an image in php.

  

#

[Official documentation page](https://www.php.net/manual/en/function.ob-clean.php)

**[To root](/README.md)**