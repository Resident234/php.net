# Unicode character properties




<div class="phpcode"><span class="html">
To select UTF-8 mode for the additional escape sequences (\p{xx}, \P{xx}, and \X) , use the &quot;u&quot; modifier (see <a href="http://php.net/manual/en/reference.pcre.pattern.modifiers.php" rel="nofollow" target="_blank">http://php.net/manual/en/reference.pcre.pattern.modifiers.php</a>).<br><br>I wondered why a German sharp S (&#xDF;) was marked as a control character by \p{Cc} and it took me a while to properly read the first sentence: &quot;Since 5.1.0, three additional escape sequences to match generic character types are available when UTF-8 mode is selected. &quot; :-$ and then to find out how to do so.</span>
</div>
  

#


<div class="phpcode"><span class="html">
My country, Vietnam, have our own alphabet table:<br><a href="http://en.wikipedia.org/wiki/Vietnamese_alphabet" rel="nofollow" target="_blank">http://en.wikipedia.org/wiki/Vietnamese_alphabet</a><br>I hope PHP will support better than in Vietnamese.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/regexp.reference.unicode.php)

**[To root](/README.md)**