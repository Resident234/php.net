# levenshtein



The levenshtein function processes each byte of the input string individually. Then for multibyte encodings, such as UTF-8, it may give misleading results.<br><br>Example with a french accented word :<br>- levenshtein(&apos;notre&apos;, &apos;votre&apos;) = 1<br>- levenshtein(&apos;notre&apos;, &apos;n&#xF4;tre&apos;) = 2 (huh ?!)<br><br>You can easily find a multibyte compliant PHP implementation of the levenshtein function but it will be of course much slower than the C implementation.<br><br>Another option is to convert the strings to a single-byte (lossless) encoding so that they can feed the fast core levenshtein function.<br><br>Here is the conversion function I used with a search engine storing UTF-8 strings, and a quick benchmark. I hope it will help.<br><br>

```
<?php
// Convert an UTF-8 encoded string to a single-byte string suitable for
// functions such as levenshtein.
// 
// The function simply uses (and updates) a tailored dynamic encoding
// (in/out map parameter) where non-ascii characters are remapped to
// the range [128-255] in order of appearance.
//
// Thus it supports up to 128 different multibyte code points max over
// the whole set of strings sharing this encoding.
//
function utf8_to_extended_ascii($str, &amp;$map)
{
    // find all multibyte characters (cf. utf-8 encoding specs)
    $matches = array();
    if (!preg_match_all('/[\xC0-\xF7][\x80-\xBF]+/', $str, $matches))
        return $str; // plain ascii string
    
    // update the encoding map with the characters not already met
    foreach ($matches[0] as $mbc)
        if (!isset($map[$mbc]))
            $map[$mbc] = chr(128 + count($map));
    
    // finally remap non-ascii characters
    return strtr($str, $map);
}

// Didactic example showing the usage of the previous conversion function but,
// for better performance, in a real application with a single input string
// matched against many strings from a database, you will probably want to
// pre-encode the input only once.
//
function levenshtein_utf8($s1, $s2)
{
    $charMap = array();
    $s1 = utf8_to_extended_ascii($s1, $charMap);
    $s2 = utf8_to_extended_ascii($s2, $charMap);
    
    return levenshtein($s1, $s2);
}
?>
```
<br><br>Results (for about 6000 calls)<br>- reference time core C function (single-byte) : 30 ms<br>- utf8 to ext-ascii conversion + core function : 90 ms<br>- full php implementation : 3000 ms  

---

[EDITOR&apos;S NOTE: original post and 2 corrections combined into 1 -- mgf]<br><br>Here is an implementation of the Levenshtein Distance calculation that only uses a one-dimensional array and doesn&apos;t have a limit to the string length. This implementation was inspired by maze generation algorithms that also use only one-dimensional arrays.<br><br>I have tested this function with two 532-character strings and it completed in 0.6-0.8 seconds. <br><br>

```
<?php
/*
* This function starts out with several checks in an attempt to save time.
*   1.  The shorter string is always used as the "right-hand" string (as the size of the array is based on its length).  
*   2.  If the left string is empty, the length of the right is returned.
*   3.  If the right string is empty, the length of the left is returned.
*   4.  If the strings are equal, a zero-distance is returned.
*   5.  If the left string is contained within the right string, the difference in length is returned.
*   6.  If the right string is contained within the left string, the difference in length is returned.
* If none of the above conditions were met, the Levenshtein algorithm is used.
*/
function LevenshteinDistance($s1, $s2)
{
  $sLeft = (strlen($s1) > strlen($s2)) ? $s1 : $s2;
  $sRight = (strlen($s1) > strlen($s2)) ? $s2 : $s1;
  $nLeftLength = strlen($sLeft);
  $nRightLength = strlen($sRight);
  if ($nLeftLength == 0)
    return $nRightLength;
  else if ($nRightLength == 0)
    return $nLeftLength;
  else if ($sLeft === $sRight)
    return 0;
  else if (($nLeftLength < $nRightLength) &amp;&amp; (strpos($sRight, $sLeft) !== FALSE))
    return $nRightLength - $nLeftLength;
  else if (($nRightLength < $nLeftLength) &amp;&amp; (strpos($sLeft, $sRight) !== FALSE))
    return $nLeftLength - $nRightLength;
  else {
    $nsDistance = range(1, $nRightLength + 1);
    for ($nLeftPos = 1; $nLeftPos <= $nLeftLength; ++$nLeftPos)
    {
      $cLeft = $sLeft[$nLeftPos - 1];
      $nDiagonal = $nLeftPos - 1;
      $nsDistance[0] = $nLeftPos;
      for ($nRightPos = 1; $nRightPos <= $nRightLength; ++$nRightPos)
      {
        $cRight = $sRight[$nRightPos - 1];
        $nCost = ($cRight == $cLeft) ? 0 : 1;
        $nNewDiagonal = $nsDistance[$nRightPos];
        $nsDistance[$nRightPos] = 
          min($nsDistance[$nRightPos] + 1, 
              $nsDistance[$nRightPos - 1] + 1, 
              $nDiagonal + $nCost);
        $nDiagonal = $nNewDiagonal;
      }
    }
    return $nsDistance[$nRightLength];
  }
}
?>
```
  

---

It&apos;s also useful if you want to make some sort of registration page and you want to make sure that people who register don&apos;t pick usernames that are very similar to their passwords.  

---

[Official documentation page](https://www.php.net/manual/en/function.levenshtein.php)

**[To root](/README.md)**