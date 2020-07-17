# metaphone



you can use the metaphone function quite effectively with phrases by taking the levenshtein distances between two metaphone codes, and then taking this as a percentage of the length of the original metaphone code. thus you can define a percentage error, (say 20%) and accept only matches that are closer than that. i&apos;ve found this works quite effectively in a function i am using on my website where an album name that the user entered is verified against existing album names that may be similar. this is also an excellent way of people being able to vaguely remember a phrase and get several suggestions out of the database. so you could type "i stiched nine times" with an error percentage of, say, 50 and still get &apos;a stitch in time saves nine&apos; back as a match.  

#

A double metaphone pecl module is available: http://pecl.php.net/package/doublemetaphone  

#

metaphone<br>=======================<br>The metaphone() function can be used for spelling applications.This function returns the metaphone key of the string on success, or FALSE on failure.Its main use is when you are searching a genealogy database. check to see if a metaphone search is offered. It is also useful in making/searching family tree.<br>Given below is a simple code that calculates and compares two strings to find whether its metaphone codes are equivalent.<br><br>html code<br>==========<br>&lt;html&gt;<br>&lt;body&gt;<br>&lt;form action="test.php" name="test" method="get"&gt;<br>Name1:&lt;input type="text" name="name1" /&gt;&lt;br /&gt;<br>Name2:&lt;input type="text" name="name2" /&gt;&lt;br /&gt;<br>&lt;input type="submit" name="submit" value="compare" /&gt;<br>&lt;/form&gt;<br><br>&lt;!--php code begins here --&gt;<br><br>

```
<?php
if(isset($_GET['submit']))
{
$str1 = $_GET['name1'];
$str2 = $_GET['name2'];
$meta_one=metaphone($str1);
$meta_two=metaphone($str2);
echo "metaphone code for ".$str1." is ". $meta_one;
echo "<br />";
echo "metaphone code for ".$str2." is ". $meta_two."<br>";
if($meta_one==$meta_two)
{
echo "metaphone codes are matching";
}
else
{
echo "metaphone codes are not matching";
}
}
?>
```
<br><br>&lt;/body&gt;<br>&lt;/html&gt;<br><br>Metaphone  algorithm was developed by Lawrence Philips.<br><br>Lawrence Philips&apos; RULES follow:<br><br> The 16 consonant sounds:<br>                                             |--- ZERO represents "th"<br>                                             |<br>      B  X  S  K  J  T  F  H  L  M  N  P  R  0  W  Y<br><br> Exceptions:<br><br>   Beginning of word: "ae-", "gn", "kn-", "pn-", "wr-"  ----&gt; drop first letter<br>                      "Aebersold", "Gnagy", "Knuth", "Pniewski", "Wright"<br><br>   Beginning of word: "x"                                ----&gt; change to "s"<br>                                      as in "Deng Xiaopeng"<br><br>   Beginning of word: "wh-"                              ----&gt; change to "w"<br>                                      as in "Whalen"<br><br> Transformations:<br><br>   B ----&gt; B      unless at the end of word after "m", as in "dumb", "McComb"<br><br>   C ----&gt; X      (sh) if "-cia-" or "-ch-"<br>           S      if "-ci-", "-ce-", or "-cy-"<br>                  SILENT if "-sci-", "-sce-", or "-scy-"<br>           K      otherwise, including in "-sch-"<br><br>   D ----&gt; J      if in "-dge-", "-dgy-", or "-dgi-"<br>           T      otherwise<br><br>   F ----&gt; F<br><br>   G ----&gt;        SILENT if in "-gh-" and not at end or before a vowel<br>                            in "-gn" or "-gned"<br>                            in "-dge-" etc., as in above rule<br>           J      if before "i", or "e", or "y" if not double "gg"<br>           K      otherwise<br><br>   H ----&gt;        SILENT if after vowel and no vowel follows<br>                         or after "-ch-", "-sh-", "-ph-", "-th-", "-gh-"<br>           H      otherwise<br><br>   J ----&gt; J<br><br>   K ----&gt;        SILENT if after "c"<br>           K      otherwise<br><br>   L ----&gt; L<br><br>   M ----&gt; M<br><br>   N ----&gt; N<br><br>   P ----&gt; F      if before "h"<br>           P      otherwise<br><br>   Q ----&gt; K<br><br>   R ----&gt; R<br><br>   S ----&gt; X      (sh) if before "h" or in "-sio-" or "-sia-"<br>           S      otherwise<br><br>   T ----&gt; X      (sh) if "-tia-" or "-tio-"<br>           0      (th) if before "h"<br>                  silent if in "-tch-"<br>           T      otherwise<br><br>   V ----&gt; F<br><br>   W ----&gt;        SILENT if not followed by a vowel<br>           W      if followed by a vowel<br><br>   X ----&gt; KS<br><br>   Y ----&gt;        SILENT if not followed by a vowel<br>           Y      if followed by a vowel<br><br>   Z ----&gt; S  

#

[Official documentation page](https://www.php.net/manual/en/function.metaphone.php)

**[To root](/README.md)**