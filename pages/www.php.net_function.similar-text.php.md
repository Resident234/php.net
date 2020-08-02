# similar_text



Hey there,<br><br>Be aware when using this function, that the order of passing the strings is very important if you want to calculate the percentage of similarity, in fact, altering the variables will give a very different result, example :<br><br>

```
<?php
$var_1 = 'PHP IS GREAT';
$var_2 = 'WITH MYSQL';

similar_text($var_1, $var_2, $percent);

echo $percent;
// 27.272727272727

similar_text($var_2, $var_1, $percent);

echo $percent;
// 18.181818181818
?>
```
  

---

Please note that this function calculates a similarity of 0 (zero) for two empty strings.<br><br>

```
<?php
similar_text("", "", $sim);
echo $sim; // "0"
?>
```
  

---

Recursive algorithm usually is very elegant one. I found a way to get better precision without the recursion. Imagine two different (or same) length ribbons with letters on each. You simply shifting one ribbon to left till it matches the letter the first.<br><br>

```
<?php

function similarity($str1, $str2) {
    $len1 = strlen($str1);
    $len2 = strlen($str2);
    
    $max = max($len1, $len2);
    $similarity = $i = $j = 0;
    
    while (($i < $len1) &amp;&amp; isset($str2[$j])) {
        if ($str1[$i] == $str2[$j]) {
            $similarity++;
            $i++;
            $j++;
        } elseif ($len1 < $len2) {
            $len1++;
            $j++;
        } elseif ($len1 > $len2) {
            $i++;
            $len1--;
        } else {
            $i++;
            $j++;
        }
    }

    return round($similarity / $max, 2);
}

$str1 = '12345678901234567890';
$str2 = '12345678991234567890';

echo 'Similarity: ' . (similarity($str1, $str2) * 100) . '%';
?>
```
  

---

Note that this function is case sensitive:<br><br>

```
<?php

$var1 = 'Hello';
$var2 = 'Hello';
$var3 = 'hello';

echo similar_text($var1, $var2);  // 5
echo similar_text($var1, $var3);  // 4?>
```
  

---

Actually similar_text() is not bad...<br>it works good. But before processing i think is a good way to make a little mod like this<br><br>$var_1 = strtoupper("doggy");<br>$var_2 = strtoupper("Dog");<br><br>similar_text($var_1, $var_2, $percent); <br><br>echo $percent; // output is 75 but without strtoupper output is 50  

---

If performance is an issue, you may wish to use the levenshtein() function instead, which has a considerably better complexity of O(str1 * str2).  

---

The speed issues for similar_text seem to be only an issue for long sections of text (&gt;20000 chars).<br><br>I found a huge performance improvement in my application by just testing if the string to be tested was less than 20000 chars before calling similar_text.<br><br>20000+ took 3-5 secs to process, anything else (10000 and below) took a fraction of a second.<br>Fortunately for me, there was only a handful of instances with &gt;20000 chars which I couldn&apos;t get a comparison % for.  

---

If you have reserved names in a database that you don&apos;t want others to use, i find this to work pretty good. <br>I added strtoupper to the variables to validate typing only. Taking case into consideration will decrease similarity. <br><br>

```
<?php
$query = mysql_query("select * from $table") or die("Query failed");

while ($row = mysql_fetch_array($query)) {
      similar_text(strtoupper($_POST['name']), strtoupper($row['reserved']), $similarity_pst);
      if (number_format($similarity_pst, 0) > 90){
        $too_similar = $row['reserved'];
        print "The name you entered is too similar the reserved name &amp;quot;".$row['reserved']."&amp;quot;";
        break;
       }
    }
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.similar-text.php)

**[To root](/README.md)**