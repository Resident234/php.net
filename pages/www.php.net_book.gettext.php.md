# Gettext





How to use gettext on Windows.

If you use Linux start from the step 2 and consider cmd as linux shell.

1] First you have to download and install this 
&#xA0; &#xA0; &#xA0; https://mlocati.github.io/articles/gettext-iconv-windows.html
&#xA0;&#xA0; 
&#xA0; &#xA0; Check all options during the installation and go on.&#xA0;&#xA0; 
 

2] Create a index.php file into your website directory with this code inside:
&#xA0; 
&#xA0;&#xA0; 

```
<?php 
&#xA0; &#xA0; echo&#xA0; _(&quot;Good Morning&quot;);
&#xA0; &#xA0; ?>
```


3] Open cmd and move into your website folder using cd

4] Now create the .mo files directly from the php files using this command
&#xA0; &#xA0; xgettext -n index.php
&#xA0;&#xA0; 
&#xA0; &#xA0; Use xgettext -help to see how to include more php files.

5] Once finished will be generate a file called messages.mo. Now you have to set the language and the charset. Open messages.mo with notepad and edit the lines:

 - &quot;Language: \n&quot; BECOMES &quot;Language fr\n&quot;
 - &quot;Content-Type: text/plain; charset=CHARSET\n&quot; BECOMES &quot;Content-Type: text/plain; charset=UTF-8\n&quot;

6] Remember to translate every line where is present msgstr &quot;&quot;, translating the instance msgid into language you have chosen. For instance:

&#xA0;&#xA0; #: index.php:2
&#xA0; msgid &quot;Good Morning&quot;
&#xA0; msgstr &quot;Bonjour&quot;

7] Once finished open cmd and move into your website folder using cd and then type
&#xA0; &#xA0;&#xA0; msgfmt messages.po

 This will make a file called messages.mo . Is a binary version of the messages.po file

8] Now you have to create a folder structure like this into your website folder. Do this for each language you want to add.
&#xA0; &#xA0; 
&#xA0; website/locale/fr_FR/LC_MESSAGES

9] Move messages.mo and messages.po in locale/fr_FR/LC_MESSAGES 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
10] Now edit the index.php as follows
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
 

```
<?php

 $locale = &quot;fr_FR&quot;;

&#xA0; &#xA0; if (defined(&apos;LC_MESSAGES&apos;)) {
&#xA0; &#xA0; &#xA0; &#xA0; setlocale(LC_MESSAGES, $locale); // Linux
&#xA0; &#xA0; &#xA0; &#xA0; bindtextdomain(&quot;messages&quot;, &quot;./locale&quot;);
&#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; putenv(&quot;LC_ALL={$locale}&quot;); // windows
&#xA0; &#xA0; &#xA0; &#xA0; bindtextdomain(&quot;messages&quot;, &quot;.\locale&quot;);
&#xA0; &#xA0; }

&#xA0; &#xA0; 
&#xA0; &#xA0; textdomain(&quot;messages&quot;);

&#xA0; &#xA0; echo _(&quot;Good Morning&quot;);
?>
```


11] Open index.php in your browser and if you will see &quot;Bonjour&quot; it means everything is okay. If not,&#xA0; start from the step 2 again.

If it was usefull for you vote up!

Visit my github profile https://github.com/TheoRelativity/

  

#



Warning for Linux (Ubuntu) users!&#xA0; Your system will *only* support the locales installed on your OS, in the *exact* format given by your OS.&#xA0; (See also the PHP setlocale man page.)&#xA0; To get a list of them, enter locale -a, which will give you something like this:

C
en_US.utf8
ja_JP.utf8
POSIX

So this machine only has English and Japanese!&#xA0; To add eg. Finnish, install the package:

sudo apt-get install language-pack-fi-base

Rerun locale -a, and &quot;fi_FI.utf8&quot; should appear.&#xA0; Make sure you&apos;re using the same name in your PHP code:

setlocale(LC_ALL, &quot;fi_FI.utf8&quot;);

Adjust your po paths so that they match, e.g. &quot;./locale/fi_FI.utf8/LC_MESSAGES/messages.po&quot;.

Now restart Apache, and it should finally work.&#xA0; Figuring this out took quite a while...

  

#



And what about pgettext and npgettext? They are there in the gettext documentation, but there aren&apos;t in PHP. They&apos;re very useful if you have the same messages for translation, but in different contexts, so they should be translated separately and probably differently.

Fortunately, there is a simple work-around, which may help:
From the gettext.h header one can find out that pgettext() is only a macro calling dcgettext() internally with properly mangled parameters - msgctxt and msgid are glued together with use of ASCII char 4 [EOT, End Of Text]. The same way they&apos;re written in .mo files, so it&apos;s possible to refer them this way.

Here&apos;s my &quot;emulated&quot; pgettext() function:



```
<?php
&#xA0;&#xA0; if (!function_exists(&apos;pgettext&apos;)) {
&#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; &#xA0; function pgettext($context, $msgid)
&#xA0; &#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $contextString = &quot;{$context}\004{$msgid}&quot;;
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; $translation = dcgettext(&apos;messages&apos;, contextString,LC_MESSAGES);
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; if ($translation == $contextString)&#xA0; return $msgid;
&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; else&#xA0; return $translation;
&#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; 
&#xA0;&#xA0; }
?>
```


By default, xgettext doesn&apos;t support pgettext function for PHP source files. But there is a parameter which can work-around it. Here&apos;s how I call xgettext:

&#xA0;&#xA0; xgettext --force-po --keyword=&quot;pgettext:1c,2&quot; -c -o messages.po sourceFile.php

In sourceFile.php I use the following test code:

&#xA0;&#xA0; pgettext(&apos;menu&apos;, &apos;Open&apos;);&#xA0; //Substitute &quot;Otw&#xF3;rz&quot;
&#xA0;&#xA0; pgettext(&apos;forum&apos;, &apos;Open&apos;);&#xA0; //Substitute &quot;Otwarty&quot;, different context

Generated .po file fragment:

&#xA0;&#xA0; msgctxt &quot;menu&quot;
&#xA0;&#xA0; msgid &quot;Open&quot;
&#xA0;&#xA0; msgstr &quot;Otw&#xF3;rz&quot;
&#xA0;&#xA0; 
&#xA0;&#xA0; msgctxt &quot;forum&quot;
&#xA0;&#xA0; msgctxt &quot;Open&quot;
&#xA0;&#xA0; msgstr &quot;Otwarty&quot;

I&apos;ve tested it out and everything works fine :-) If anyone have some further suggestions or fixes, please write ;-)

  

#



*An important thing to keep in mind*:
Do not forget to set the charset in .po file! 
For example:

&quot;Content-Type: text/plain; charset=UTF-8\n&quot;

Then PHP will be able to find the .mo file you generated, using msgfmt, from the .po file WITH CHARSET SET.

Because of this I&apos;ve wasted a lot of time debugging my code, testing every single little changes people suggested over this manual and Internet: 



```
<?php

//this:
setlocale( LC_MESSAGES, &apos;pt_BR&apos;)
//or this:
setlocale( LC_MESSAGES, &apos;pt_BR.utf8&apos;)
//or this:
setlocale( LC_MESSAGES, &apos;&apos;)

//this:
putenv(&quot;LANG=pt_BR.utf8&quot;);
//or this:
putenv(&quot;LANGUAGE=pt_BR.utf8&quot;);

//this:
bindtextdomain(&apos;mydomain&apos;, dirname(__FILE__).&apos;/locale&apos;);
//or this:
bindtextdomain(&quot;*&quot;, dirname(__FILE__).&apos;/locale&apos;);
//or this:
bindtextdomain(&apos;*&apos;, dirname(__FILE__).&apos;/locale&apos;);

//setting or not &quot;bind_textdomain_codeset()&quot;:
bind_textdomain_codeset(&quot;mydomain&quot;, &apos;UTF-8&apos;);
?>
```


As well as what locale directory name to set:
./locale/pt_BR.UTF8/LC_MESSAGES/mydomain.mo
or
./locale/pt_BR/LC_MESSAGES/mydomain.mo
or
./locale/pt/LC_MESSAGES/mydomain.mo

Finally, the code which brought the right translated strings (also with the correct charset) was:



```
<?php

$directory = dirname(__FILE__).&apos;/locale&apos;;
$domain = &apos;mydomain&apos;;
$locale =&quot;pt_BR.utf8&quot;;

//putenv(&quot;LANG=&quot;.$locale); //not needed for my tests, but people say it&apos;s useful for windows

setlocale( LC_MESSAGES, $locale);
bindtextdomain($domain, $directory);
textdomain($domain);
bind_textdomain_codeset($domain, &apos;UTF-8&apos;);
?>
```


And the three directory&apos;s names worked out, using the pt_BR.utf8 locale. (My tests were made restarting Apache then trying each directory).

I hope to help someone else not to waste as much time as I&apos;ve wasted... =P

Using:
Ubuntu 8.04 (hardy)
Apache 2.2.8
PHP 5.2.4-2ubuntu5.6

  

#

[Official documentation page](https://www.php.net/manual/en/book.gettext.php)

**[To root](/README.md)**