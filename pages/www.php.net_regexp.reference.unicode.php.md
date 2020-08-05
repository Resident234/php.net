# Unicode character properties



To select UTF-8 mode for the additional escape sequences (\p{xx}, \P{xx}, and \X) , use the "u" modifier (see http://php.net/manual/en/reference.pcre.pattern.modifiers.php).<br><br>I wondered why a German sharp S (&#xDF;) was marked as a control character by \p{Cc} and it took me a while to properly read the first sentence: "Since 5.1.0, three additional escape sequences to match generic character types are available when UTF-8 mode is selected. " :-$ and then to find out how to do so.  

---

My country, Vietnam, have our own alphabet table:<br>http://en.wikipedia.org/wiki/Vietnamese_alphabet<br>I hope PHP will support better than in Vietnamese.  

---

[Official documentation page](https://www.php.net/manual/en/regexp.reference.unicode.php)

**[To root](/README.md)**