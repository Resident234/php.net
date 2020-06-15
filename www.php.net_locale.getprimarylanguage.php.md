# Locale::getPrimaryLanguagelocale_get_primary_language




<div class="phpcode"><span class="html">
The behaviour when a falsy value is passed as the $locale is undocumented, but it appears that it returns the primary language of the default system language. In my case:<br><br>&#xA0; &#xA0; Locale::getPrimaryLanguage(null);<br><br>Returns &apos;en&apos;. So make sure to test $locale before passing it to the method.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/locale.getprimarylanguage.php)

**[To root](/README.md)**