# iconv



The "//ignore" option doesn&apos;t work with recent versions of the iconv library.  So if you&apos;re having trouble with that option, you aren&apos;t alone.  <br><br>That means you can&apos;t currently use this function to filter invalid characters.  Instead it silently fails and returns an empty string (or you&apos;ll get a notice but only if you have E_NOTICE enabled).<br><br>This has been a known bug with a known solution for at least since 2009 years but no one seems to be willing to fix it (PHP must pass the -c option to iconv).  It&apos;s still broken as of the latest release 5.4.3. <br><br>https://bugs.php.net/bug.php?id=48147<br>https://bugs.php.net/bug.php?id=52211<br>https://bugs.php.net/bug.php?id=61484<br><br>[UPDATE 15-JUN-2012]<br>Here&apos;s a workaround...<br><br>  ini_set(&apos;mbstring.substitute_character&apos;, "none");<br>  $text= mb_convert_encoding($text, &apos;UTF-8&apos;, &apos;UTF-8&apos;);<br><br>That will strip invalid characters from UTF-8 strings (so that you can insert it into a database, etc.).  Instead of "none" you can also use the value 32 if you want it to insert spaces in place of the invalid characters.  

---

Please note that iconv(&apos;UTF-8&apos;, &apos;ASCII//TRANSLIT&apos;, ...) doesn&apos;t work properly when locale category LC_CTYPE is set to C or POSIX. You must choose another locale otherwise all non-ASCII characters will be replaced with question marks. This is at least true with glibc 2.5.<br><br>Example:<br>

```
<?php
setlocale(LC_CTYPE, 'POSIX');
echo iconv('UTF-8', 'ASCII//TRANSLIT', "&#x17D;lu&#x165;ou&#x10D;k&#xFD; k&#x16F;&#x148;\n");
// ?lu?ou?k? k??

setlocale(LC_CTYPE, 'cs_CZ');
echo iconv('UTF-8', 'ASCII//TRANSLIT', "&#x17D;lu&#x165;ou&#x10D;k&#xFD; k&#x16F;&#x148;\n");
// Zlutoucky kun
?>
```
  

---

Interestingly, setting different target locales results in different, yet appropriate, transliterations. For example:<br><br>

```
<?php
//some German
$utf8_sentence = 'Wei&#xDF;, Goldmann, G&#xF6;bel, Weiss, G&#xF6;the, Goethe und G&#xF6;tz';

//UK
setlocale(LC_ALL, 'en_GB');

//transliterate
$trans_sentence = iconv('UTF-8', 'ASCII//TRANSLIT', $utf8_sentence);

//gives [Weiss, Goldmann, Gobel, Weiss, Gothe, Goethe und Gotz]
//which is our original string flattened into 7-bit ASCII as
//an English speaker would do it (ie. simply remove the umlauts)
echo $trans_sentence . PHP_EOL;

//Germany
setlocale(LC_ALL, 'de_DE');

$trans_sentence = iconv('UTF-8', 'ASCII//TRANSLIT', $utf8_sentence);

//gives [Weiss, Goldmann, Goebel, Weiss, Goethe, Goethe und Goetz]
//which is exactly how a German would transliterate those
//umlauted characters if forced to use 7-bit ASCII!
//(because really &#xE4; = ae, &#xF6; = oe and &#xFC; = ue)
echo $trans_sentence . PHP_EOL;

?>
```
  

---

to test different combinations of convertions between charsets (when we don&apos;t know the source charset and what is the convenient destination charset) this is an example :<br><br>

```
<?php
$tab = array("UTF-8", "ASCII", "Windows-1252", "ISO-8859-15", "ISO-8859-1", "ISO-8859-6", "CP1256");
$chain = "";
foreach ($tab as $i)
    {
        foreach ($tab as $j)
        {
            $chain .= " $i$j ".iconv($i, $j, "$my_string");
        }
    }

echo $chain;
?>
```
<br><br>then after displaying, you use the $i$j that shows good displaying.<br>NB: you can add other charsets to $tab  to test other cases.  

---

If you are getting question-marks in your iconv output when transliterating, be sure to &apos;setlocale&apos; to something your system supports.<br><br>Some PHP CMS&apos;s will default setlocale to &apos;C&apos;, this can be a problem.<br><br>use the "locale" command to find out a list..<br><br>$ locale -a<br>C<br>en_AU.utf8<br>POSIX<br><br>

```
<?php
  setlocale(LC_CTYPE, 'en_AU.utf8');
  $str = iconv('UTF-8', 'ASCII//TRANSLIT', "C&#xF4;te d'Ivoire");
?>
```
  

---

Like many other people, I have encountered massive problems when using iconv() to convert between encodings (from UTF-8 to ISO-8859-15 in my case), especially on large strings.<br><br>The main problem here is that when your string contains illegal UTF-8 characters, there is no really straight forward way to handle those. iconv() simply (and silently!) terminates the string when encountering the problematic characters (also if using //IGNORE), returning a clipped string. The<br><br>

```
<?php

$newstring = html_entity_decode(htmlentities($oldstring, ENT_QUOTES, 'UTF-8'), ENT_QUOTES , 'ISO-8859-15');

?>
```


workaround suggested here and elsewhere will also break when encountering illegal characters, at least dropping a useful note ("htmlentities(): Invalid multibyte sequence in argument in...")

I have found a lot of hints, suggestions and alternative methods (it's scary and in my opinion no good sign how many ways PHP natively provides to convert the encoding of strings), but none of them really worked, except for this one:



```
<?php

$newstring = mb_convert_encoding($oldstring, 'ISO-8859-15', 'UTF-8');

?>
```
  

---

There may be situations when a new version of a web site, all in UTF-8, has to display some old data remaining in the database with ISO-8859-1 accents. The problem is iconv("ISO-8859-1", "UTF-8", $string) should not be applied if $string is already UTF-8 encoded.<br><br>I use this function that does&apos;nt need any extension :<br><br>function convert_utf8( $string ) { <br>    if ( strlen(utf8_decode($string)) == strlen($string) ) {   <br>        // $string is not UTF-8<br>        return iconv("ISO-8859-1", "UTF-8", $string);<br>    } else {<br>        // already UTF-8<br>        return $string;<br>    }<br>}<br><br> I have not tested it extensively, hope it may help.  

---

I have used iconv to convert from cp1251 into UTF-8. I spent a day to investigate why a string with Russian capital &apos;&#x420;&apos; (sounds similar to &apos;r&apos;) at the end cannot be inserted into a database.<br><br>The problem is not in iconv. But &apos;&#x420;&apos; in cp1251 is chr(208) and &apos;&#x420;&apos; in UTF-8 is chr(208).chr(106). chr(106) is one of the space symbol which match &apos;\s&apos; in regex. So, it can be taken by a greedy &apos;+&apos; or &apos;*&apos; operator. In that case, you loose &apos;&#x420;&apos; in your string.<br><br>For example, &apos;&#x413;&#x420;   &apos; (Russian, UTF-8). Function preg_match. Regex is &apos;(.+?)[\s]*&apos;. Then &apos;(.+?)&apos; matches &apos;&#x413;&apos;.chr(208) and &apos;[\s]*&apos; matches chr(106).&apos;   &apos;.<br><br>Although, it is not a bug of iconv, but it looks like it very much. That&apos;s why I put this comment here.  

---

[Official documentation page](https://www.php.net/manual/en/function.iconv.php)

**[To root](/README.md)**