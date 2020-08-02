# Gettext



How to use gettext on Windows.<br><br>If you use Linux start from the step 2 and consider cmd as linux shell.<br><br>1] First you have to download and install this <br>      https://mlocati.github.io/articles/gettext-iconv-windows.html<br>   <br>    Check all options during the installation and go on.   <br> <br><br>2] Create a index.php file into your website directory with this code inside:<br>  <br>   

```
<?php 
    echo  _("Good Morning");
    ?>
```


3] Open cmd and move into your website folder using cd

4] Now create the .mo files directly from the php files using this command
    xgettext -n index.php
   
    Use xgettext -help to see how to include more php files.

5] Once finished will be generate a file called messages.mo. Now you have to set the language and the charset. Open messages.mo with notepad and edit the lines:

 - "Language: \n" BECOMES "Language fr\n"
 - "Content-Type: text/plain; charset=CHARSET\n" BECOMES "Content-Type: text/plain; charset=UTF-8\n"

6] Remember to translate every line where is present msgstr "", translating the instance msgid into language you have chosen. For instance:

   #: index.php:2
  msgid "Good Morning"
  msgstr "Bonjour"

7] Once finished open cmd and move into your website folder using cd and then type
     msgfmt messages.po

 This will make a file called messages.mo . Is a binary version of the messages.po file

8] Now you have to create a folder structure like this into your website folder. Do this for each language you want to add.
    
  website/locale/fr_FR/LC_MESSAGES

9] Move messages.mo and messages.po in locale/fr_FR/LC_MESSAGES 
                
10] Now edit the index.php as follows
                                        
 

```
<?php

 $locale = "fr_FR";

    if (defined('LC_MESSAGES')) {
        setlocale(LC_MESSAGES, $locale); // Linux
        bindtextdomain("messages", "./locale");
    } else {
        putenv("LC_ALL={$locale}"); // windows
        bindtextdomain("messages", ".\locale");
    }

    
    textdomain("messages");

    echo _("Good Morning");
?>
```
<br><br>11] Open index.php in your browser and if you will see "Bonjour" it means everything is okay. If not,  start from the step 2 again.<br><br>If it was usefull for you vote up!<br><br>Visit my github profile https://github.com/TheoRelativity/  

---

Warning for Linux (Ubuntu) users!  Your system will *only* support the locales installed on your OS, in the *exact* format given by your OS.  (See also the PHP setlocale man page.)  To get a list of them, enter locale -a, which will give you something like this:<br><br>C<br>en_US.utf8<br>ja_JP.utf8<br>POSIX<br><br>So this machine only has English and Japanese!  To add eg. Finnish, install the package:<br><br>sudo apt-get install language-pack-fi-base<br><br>Rerun locale -a, and "fi_FI.utf8" should appear.  Make sure you&apos;re using the same name in your PHP code:<br><br>setlocale(LC_ALL, "fi_FI.utf8");<br><br>Adjust your po paths so that they match, e.g. "./locale/fi_FI.utf8/LC_MESSAGES/messages.po".<br><br>Now restart Apache, and it should finally work.  Figuring this out took quite a while...  

---

And what about pgettext and npgettext? They are there in the gettext documentation, but there aren&apos;t in PHP. They&apos;re very useful if you have the same messages for translation, but in different contexts, so they should be translated separately and probably differently.<br><br>Fortunately, there is a simple work-around, which may help:<br>From the gettext.h header one can find out that pgettext() is only a macro calling dcgettext() internally with properly mangled parameters - msgctxt and msgid are glued together with use of ASCII char 4 [EOT, End Of Text]. The same way they&apos;re written in .mo files, so it&apos;s possible to refer them this way.<br><br>Here&apos;s my "emulated" pgettext() function:<br><br>

```
<?php
   if (!function_exists('pgettext')) {
      
      function pgettext($context, $msgid)
      {
         $contextString = "{$context}\004{$msgid}";
         $translation = dcgettext('messages', contextString,LC_MESSAGES);
         if ($translation == $contextString)  return $msgid;
         else  return $translation;
      }
      
   }
?>
```
<br><br>By default, xgettext doesn&apos;t support pgettext function for PHP source files. But there is a parameter which can work-around it. Here&apos;s how I call xgettext:<br><br>   xgettext --force-po --keyword="pgettext:1c,2" -c -o messages.po sourceFile.php<br><br>In sourceFile.php I use the following test code:<br><br>   pgettext(&apos;menu&apos;, &apos;Open&apos;);  //Substitute "Otw&#xF3;rz"<br>   pgettext(&apos;forum&apos;, &apos;Open&apos;);  //Substitute "Otwarty", different context<br><br>Generated .po file fragment:<br><br>   msgctxt "menu"<br>   msgid "Open"<br>   msgstr "Otw&#xF3;rz"<br>   <br>   msgctxt "forum"<br>   msgctxt "Open"<br>   msgstr "Otwarty"<br><br>I&apos;ve tested it out and everything works fine :-) If anyone have some further suggestions or fixes, please write ;-)  

---

*An important thing to keep in mind*:<br>Do not forget to set the charset in .po file! <br>For example:<br><br>"Content-Type: text/plain; charset=UTF-8\n"<br><br>Then PHP will be able to find the .mo file you generated, using msgfmt, from the .po file WITH CHARSET SET.<br><br>Because of this I&apos;ve wasted a lot of time debugging my code, testing every single little changes people suggested over this manual and Internet: <br><br>

```
<?php

//this:
setlocale( LC_MESSAGES, 'pt_BR')
//or this:
setlocale( LC_MESSAGES, 'pt_BR.utf8')
//or this:
setlocale( LC_MESSAGES, '')

//this:
putenv("LANG=pt_BR.utf8");
//or this:
putenv("LANGUAGE=pt_BR.utf8");

//this:
bindtextdomain('mydomain', dirname(__FILE__).'/locale');
//or this:
bindtextdomain("*", dirname(__FILE__).'/locale');
//or this:
bindtextdomain('*', dirname(__FILE__).'/locale');

//setting or not "bind_textdomain_codeset()":
bind_textdomain_codeset("mydomain", 'UTF-8');
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

$directory = dirname(__FILE__).'/locale';
$domain = 'mydomain';
$locale ="pt_BR.utf8";

//putenv("LANG=".$locale); //not needed for my tests, but people say it's useful for windows

setlocale( LC_MESSAGES, $locale);
bindtextdomain($domain, $directory);
textdomain($domain);
bind_textdomain_codeset($domain, 'UTF-8');
?>
```
<br><br>And the three directory&apos;s names worked out, using the pt_BR.utf8 locale. (My tests were made restarting Apache then trying each directory).<br><br>I hope to help someone else not to waste as much time as I&apos;ve wasted... =P<br><br>Using:<br>Ubuntu 8.04 (hardy)<br>Apache 2.2.8<br>PHP 5.2.4-2ubuntu5.6  

---

[Official documentation page](https://www.php.net/manual/en/book.gettext.php)

**[To root](/README.md)**