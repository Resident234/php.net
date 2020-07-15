# jdtojewish





```
<?php
// Hebrew date in hebrew
$str = jdtojewish(gregoriantojd( date('m'), date('d'), date('Y')), true, CAL_JEWISH_ADD_GERESHAYIM + CAL_JEWISH_ADD_ALAFIM + CAL_JEWISH_ADD_ALAFIM_GERESH); // for today
$str1 = iconv ('WINDOWS-1255', 'UTF-8', $str); // convert to utf-8

echo $str1; // for 23/03/2012 will print: &#x5DB;"&#x5D8; &#x5D0;&#x5D3;&#x5E8; &#x5D4;' &#x5D0;&#x5DC;&#x5E4;&#x5D9;&#x5DD; &#x5EA;&#x5E9;&#x5E2;"&#x5D1;

// or
$str = jdtojewish(gregoriantojd( date('m'), date('d'), date('Y')), true, CAL_JEWISH_ADD_GERESHAYIM); // for today
$str1 = iconv ('WINDOWS-1255', 'UTF-8', $str); // convert to utf-8

echo $str1; // for 23/03/2012 will print: &#x5DB;"&#x5D8; &#x5D0;&#x5D3;&#x5E8; &#x5D4;&#x5EA;&#x5E9;&#x5E2;"&#x5D1;
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.jdtojewish.php)

**[To root](/README.md)**