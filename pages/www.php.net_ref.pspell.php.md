# Pspell Functions



preg_replace_callback() is the best way I found to check words in a large text and change the HTML color of words that are not found.  Here is a script I wrote which does just that, and optionally uses pspell_suggest() to auto-correct. I would love to write a better regex to ignore &lt;text inside xml tags&gt;, but that really isn&apos;t as easy as it sounds...<br>

```
<?php
$pspell = pspell_new('en','canadian','','utf-8',PSPELL_FAST);

function spellCheckWord($word) {
    global $pspell;
    $autocorrect = TRUE;

    // Take the string match from preg_replace_callback's array
    $word = $word[0];
    
    // Ignore ALL CAPS
    if (preg_match('/^[A-Z]*$/',$word)) return $word;

    // Return dictionary words
    if (pspell_check($pspell,$word))
        return $word;

    // Auto-correct with the first suggestion, color green
    if ($autocorrect &amp;&amp; $suggestions = pspell_suggest($pspell,$word))
        return '<span style="color:#00FF00;">'.current($suggestions).'</span>';
    
    // No suggestions, color red
    return '<span style="color:#FF0000;">'.$word.'</span>';
}

function spellCheck($string) {
    return preg_replace_callback('/\b\w+\b/','spellCheckWord',$string);
}

echo spellCheck('PHP is a reflecktive proegramming langwage origenaly dezigned for prodewcing dinamic waieb pagges.');
?>
```
<br>OUTPUT: PHP is a reflective programming language originally designed for producing dynamic Web pages.  

#

[Official documentation page](https://www.php.net/manual/en/ref.pspell.php)

**[To root](/README.md)**