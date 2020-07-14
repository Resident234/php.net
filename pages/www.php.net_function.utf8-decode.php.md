# utf8_decode



Please note that utf8_decode simply converts a string encoded in UTF-8 to ISO-8859-1. A more appropriate name for it would be utf8_to_iso88591. If your text is already encoded in ISO-8859-1, you do not need this function. If you don&apos;t want to use ISO-8859-1, you do not need this function.<br><br>Note that UTF-8 can represent many more characters than ISO-8859-1. Trying to convert a UTF-8 string that contains characters that can&apos;t be represented in ISO-8859-1 to ISO-8859-1 will garble your text and/or cause characters to go missing. Trying to convert text that is not encoded in UTF-8 using this function will most likely garble the text.<br><br>If you need to convert any text from any encoding to any other encoding, look at iconv() instead.  

#

[Official documentation page](https://www.php.net/manual/en/function.utf8-decode.php)

**[To root](/README.md)**