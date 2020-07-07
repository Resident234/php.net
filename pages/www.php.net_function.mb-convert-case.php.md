# mb_convert_case





as the previouly posted version of this function doesn&apos;t handle UTF-8 characters, I simply tried to replace ucfirst to mb_convert_case, but then any previous case foldings were lost while looping through delimiters. 
So I decided to do an mb_convert_case on the input string (it also deals with words is uppercase wich may also be problematic when doing case-sensitive search), and do the rest of checking after that.

As with mb_convert_case, words are capitalized, I also added lowercase convertion for the exceptions, but, for the above mentioned reason, I left ucfirst unchanged.

Now it works fine for utf-8 strings as well, except for string delimiters followed by an UTF-8 character (&quot;Mc&#xE1;d&#xE1;m&quot; is unchanged, while &quot;mcdunno&apos;s&quot; is converted to &quot;McDunno&apos;s&quot; and &quot;&#xF6;kr&#xF6;s-T&#xD3;TH &#xE9;DUa&quot; in also put in the correct form)

I use it for checking user input on names and addresses, so exceptions list contains some hungarian words too.



```
<?php

function titleCase($string, $delimiters = array(&quot; &quot;, &quot;-&quot;, &quot;.&quot;, &quot;&apos;&quot;, &quot;O&apos;&quot;, &quot;Mc&quot;), $exceptions = array(&quot;&#xFA;t&quot;, &quot;u&quot;, &quot;s&quot;, &quot;&#xE9;s&quot;, &quot;utca&quot;, &quot;t&#xE9;r&quot;, &quot;krt&quot;, &quot;k&#xF6;r&#xFA;t&quot;, &quot;s&#xE9;t&#xE1;ny&quot;, &quot;I&quot;, &quot;II&quot;, &quot;III&quot;, &quot;IV&quot;, &quot;V&quot;, &quot;VI&quot;, &quot;VII&quot;, &quot;VIII&quot;, &quot;IX&quot;, &quot;X&quot;, &quot;XI&quot;, &quot;XII&quot;, &quot;XIII&quot;, &quot;XIV&quot;, &quot;XV&quot;, &quot;XVI&quot;, &quot;XVII&quot;, &quot;XVIII&quot;, &quot;XIX&quot;, &quot;XX&quot;, &quot;XXI&quot;, &quot;XXII&quot;, &quot;XXIII&quot;, &quot;XXIV&quot;, &quot;XXV&quot;, &quot;XXVI&quot;, &quot;XXVII&quot;, &quot;XXVIII&quot;, &quot;XXIX&quot;, &quot;XXX&quot; )) {
&#xA0; &#xA0; &#xA0;&#xA0; /*
&#xA0; &#xA0; &#xA0; &#xA0; * Exceptions in lower case are words you don&apos;t want converted
&#xA0; &#xA0; &#xA0; &#xA0; * Exceptions all in upper case are any words you don&apos;t want converted to title case
&#xA0; &#xA0; &#xA0; &#xA0; *&#xA0;&#xA0; but should be converted to upper case, e.g.:
&#xA0; &#xA0; &#xA0; &#xA0; *&#xA0;&#xA0; king henry viii or king henry Viii should be King Henry VIII
&#xA0; &#xA0; &#xA0; &#xA0; */
&#xA0; &#xA0; &#xA0; &#xA0; $string = mb_convert_case($string, MB_CASE_TITLE, &quot;UTF-8&quot;);

&#xA0; &#xA0; &#xA0;&#xA0; foreach ($delimiters as $dlnr =&gt; $delimiter){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $words = explode($delimiter, $string);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $newwords = array();
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; foreach ($words as $wordnr =&gt; $word){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; if (in_array(mb_strtoupper($word, &quot;UTF-8&quot;), $exceptions)){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // check exceptions list for any words that should be in upper case
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $word = mb_strtoupper($word, &quot;UTF-8&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; elseif (in_array(mb_strtolower($word, &quot;UTF-8&quot;), $exceptions)){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // check exceptions list for any words that should be in upper case
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $word = mb_strtolower($word, &quot;UTF-8&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; elseif (!in_array($word, $exceptions) ){
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; // convert to uppercase (non-utf8 only)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $word = ucfirst($word);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; array_push($newwords, $word);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $string = join($delimiter, $newwords);
&#xA0; &#xA0; &#xA0;&#xA0; }//foreach
&#xA0; &#xA0; &#xA0;&#xA0; return $string;
} 

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.mb-convert-case.php)

**[To root](/README.md)**