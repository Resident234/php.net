# Gettext Functions





As some of you have mentioned, you must have the locales compiled on your system first. How this is done is depending on your system/distribution *sigh*

In every case you need to have gettext installed on your system. Users running a Linux with a recent version of libc will have gettext in libc already.

Here&apos;s an example for those of you running Ubuntu Edgy.

First of all, have a look in /usr/share/i18n/SUPPORTED, here are the locales that are supported on your system. To compile a locale we will use the command &quot;locale-gen&quot;. (&quot;man 8 locale-gen&quot; is good reading)

This command reads configuration from files in the folder /var/lib/locales/supported.d/ In these files the locales and charsets are defined.

If we take swedish as an example, start by creating a file called &quot;sv&quot; in /var/lib/locales/supported.d/ and then put in something like &quot;sv_SE.UTF8 UTF8&quot; (without the &quot; &quot;) This says that we want the locale sv_SE to be built with charmap UTF-8. Check the file /usr/share/i18n/SUPPORTED for other alternatives.

Now we run &quot;locale-gen&quot; which will compile all locales defined in /var/lib/locales/supported.d/ and put them in /usr/lib/locales (yes that&apos;s a lot of locations...)

Now you&apos;re ready to go...

Create a php-file (e.g. hello.php) where all strings you want to translate are surrounded with _(&quot;string here&quot;);

next run &quot;xgettext hello.php&quot;, this creates a file called messages.po, which is you translation-file (pot). Change the &quot;Content-Type: text/plain; charset=CHARSET\n&quot; in this file and replace CHARSET with UTF-8 in this case. Next translate all strings like this.

#: hello.php:3
msgid &quot;hello!&quot;
msgstr &quot;hej!&quot;

Save and use the command &quot;msgfmt -o hello.mo messages.po&quot; which will create a compiled .mo file called hello.mo

This file is then placed in a directory structure somewhere your Apache can read it. The structure looks like this:

&quot;locale/sv/LC_MESSAGES/hello.mo&quot;

Next, let php know what we&apos;re doing. We point to where we have our translations. Add the following at the top of hello.php

bindtextdomain(&apos;hello&apos;,&apos;/somepath/locale&apos;);

This binds the file hello.mo to the locale/ directory you created.
Then set the locale. LC_ALL tells that we want everything translated. Might not be good at all times, as another person here suggested.

setlocale(LC_ALL, &quot;sv_SE.UTF-8&quot;);

Finally select the that we want to use hello.mo 

textdomain(&apos;hello&apos;);

That should be it! Try it out..

If you get any problems try restarting Apache as it seems to cache the locales.

If you are running command line you might have to set the environment variable LANGUAGE to your locale as well. I didn&apos;t have to do that to get it working in apache though, but you might...

putenv(&quot;LANGUAGE=sv_SE.UTF-8&quot;);

  

#

[Official documentation page](https://www.php.net/manual/en/ref.gettext.php)

**[To root](/README.md)**