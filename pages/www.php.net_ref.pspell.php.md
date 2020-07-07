# Pspell Functions





preg_replace_callback() is the best way I found to check words in a large text and change the HTML color of words that are not found.&#xA0; Here is a script I wrote which does just that, and optionally uses pspell_suggest() to auto-correct. I would love to write a better regex to ignore &lt;text inside xml tags&gt;, but that really isn&apos;t as easy as it sounds...


```
<?php
$pspell = pspell_new(&apos;en&apos;,&apos;canadian&apos;,&apos;&apos;,&apos;utf-8&apos;,PSPELL_FAST);

function spellCheckWord($word) {
&#xA0; &#xA0; global $pspell;
&#xA0; &#xA0; $autocorrect = TRUE;

&#xA0; &#xA0; // Take the string match from preg_replace_callback&apos;s array
&#xA0; &#xA0; $word = $word[0];
&#xA0; &#xA0; 
&#xA0; &#xA0; // Ignore ALL CAPS
&#xA0; &#xA0; if (preg_match(&apos;/^[A-Z]*$/&apos;,$word)) return $word;

&#xA0; &#xA0; // Return dictionary words
&#xA0; &#xA0; if (pspell_check($pspell,$word))
&#xA0; &#xA0; &#xA0; &#xA0; return $word;

&#xA0; &#xA0; // Auto-correct with the first suggestion, color green
&#xA0; &#xA0; if ($autocorrect &amp;&amp; $suggestions = pspell_suggest($pspell,$word))
&#xA0; &#xA0; &#xA0; &#xA0; return &apos;&lt;span style=&quot;color:#00FF00;&quot;&gt;&apos;.current($suggestions).&apos;&lt;/span&gt;&apos;;
&#xA0; &#xA0; 
&#xA0; &#xA0; // No suggestions, color red
&#xA0; &#xA0; return &apos;&lt;span style=&quot;color:#FF0000;&quot;&gt;&apos;.$word.&apos;&lt;/span&gt;&apos;;
}

function spellCheck($string) {
&#xA0; &#xA0; return preg_replace_callback(&apos;/\b\w+\b/&apos;,&apos;spellCheckWord&apos;,$string);
}

echo spellCheck(&apos;PHP is a reflecktive proegramming langwage origenaly dezigned for prodewcing dinamic waieb pagges.&apos;);
?>
```

OUTPUT: PHP is a reflective programming language originally designed for producing dynamic Web pages.

  

#

[Official documentation page](https://www.php.net/manual/en/ref.pspell.php)

**[To root](/README.md)**