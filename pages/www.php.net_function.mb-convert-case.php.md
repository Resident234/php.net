# mb_convert_case



as the previouly posted version of this function doesn&apos;t handle UTF-8 characters, I simply tried to replace ucfirst to mb_convert_case, but then any previous case foldings were lost while looping through delimiters. <br>So I decided to do an mb_convert_case on the input string (it also deals with words is uppercase wich may also be problematic when doing case-sensitive search), and do the rest of checking after that.<br><br>As with mb_convert_case, words are capitalized, I also added lowercase convertion for the exceptions, but, for the above mentioned reason, I left ucfirst unchanged.<br><br>Now it works fine for utf-8 strings as well, except for string delimiters followed by an UTF-8 character ("Mc&#xE1;d&#xE1;m" is unchanged, while "mcdunno&apos;s" is converted to "McDunno&apos;s" and "&#xF6;kr&#xF6;s-T&#xD3;TH &#xE9;DUa" in also put in the correct form)<br><br>I use it for checking user input on names and addresses, so exceptions list contains some hungarian words too.<br><br>

```
<?php

function titleCase($string, $delimiters = array(" ", "-", ".", "'", "O'", "Mc"), $exceptions = array("&#xFA;t", "u", "s", "&#xE9;s", "utca", "t&#xE9;r", "krt", "k&#xF6;r&#xFA;t", "s&#xE9;t&#xE1;ny", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX", "X", "XI", "XII", "XIII", "XIV", "XV", "XVI", "XVII", "XVIII", "XIX", "XX", "XXI", "XXII", "XXIII", "XXIV", "XXV", "XXVI", "XXVII", "XXVIII", "XXIX", "XXX" )) {
       /*
        * Exceptions in lower case are words you don't want converted
        * Exceptions all in upper case are any words you don't want converted to title case
        *   but should be converted to upper case, e.g.:
        *   king henry viii or king henry Viii should be King Henry VIII
        */
        $string = mb_convert_case($string, MB_CASE_TITLE, "UTF-8");

       foreach ($delimiters as $dlnr => $delimiter){
               $words = explode($delimiter, $string);
               $newwords = array();
               foreach ($words as $wordnr => $word){
               
                       if (in_array(mb_strtoupper($word, "UTF-8"), $exceptions)){
                               // check exceptions list for any words that should be in upper case
                               $word = mb_strtoupper($word, "UTF-8");
                       }
                       elseif (in_array(mb_strtolower($word, "UTF-8"), $exceptions)){
                               // check exceptions list for any words that should be in upper case
                               $word = mb_strtolower($word, "UTF-8");
                       }
                       
                       elseif (!in_array($word, $exceptions) ){
                               // convert to uppercase (non-utf8 only)
                             
                               $word = ucfirst($word);
                               
                       }
                       array_push($newwords, $word);
               }
               $string = join($delimiter, $newwords);
       }//foreach
       return $string;
} 

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.mb-convert-case.php)

**[To root](/README.md)**