# idn_to_ascii





The notes on this function are not very clear and a little misleading.



Firstly, &lt;=5.3, you will need to make use of one of several scripts or classes available on the internet which might, or might not, require the installation of of the intl and idn PECL extensions ...and you will need to have !&lt;4.0 in order to be able to install both.



Secondly, if you have &gt;=5.4 you will not require the PECL extensions.



Third, use of utf8_encode() is not necessary.&#xA0; In fact, it will potentially prevent idn_to_ascii() from working at all.



On my setup it was necessary to change the charset in the script meta tags to UTF-8:



&lt;meta http-equiv=&quot;Content-Type&quot; content=&quot;text/html; charset=UTF-8&quot; /&gt;



...and to change charset_default in the php.ini file (/usr/local/lib/php.ini, whereis php.ini, find / -name php.ini):



default_charset = &quot;UTF-8&quot;



The above changes mean that idn_to_ascii() can now be used with that syntax (no need for utf8_encode()).&#xA0; Previously, the function worked to convert some IDNs, but failed to convert Japanese and Cyrillic IDNs.&#xA0; Further, no additional locales were enabled or added, and Apache&apos;s charset file was left unmodified.



It is also important to remember only to apply the function where required, eg:



idn_to_ascii(c&#xE5;sino.com) // is wrong



...whereas...



iden_to_ascii(c&#xE5;sino) // is right



...and also be aware of text editors that don&apos;t support UTF-8 encoding, or the $domain = &apos;c&#xE5;sino&apos; value will end up as $domain = &apos;??????&apos; ...and the function will fail.



I have found that Notepad++ easily and reliably handles UTF-8 encoding that works for this function using UTF-8 as the encoding option, not UTF-8 without BOM.

  

#

[Official documentation page](https://www.php.net/manual/en/function.idn-to-ascii.php)

**[To root](/README.md)**