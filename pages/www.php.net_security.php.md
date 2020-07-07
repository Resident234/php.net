# Security





If a single file has to be included than I use the following

index.php ( where the file is gonna be included )
___________


```
<?php
&#xA0; &#xA0; define(&apos;thefooter&apos;, TRUE);
&#xA0; &#xA0; include(&apos;folder/footer.inc.php&apos;);
?>
```


and the footer file (for example) looks this way then

footer.inc.php ( the file to be inluded )
___________


```
<?php
&#xA0; &#xA0; defined(&apos;thefooter&apos;) or die(&apos;Not with me my friend&apos;);
&#xA0; &#xA0; echo(&apos;Copyright to me in the year 2000&apos;);
?>
```


So when someone tries to access the footer.php file directly he/she/it will get the &quot;Not with me my friend&quot; messages written on the screen. An alternative option is to redirect the person who wants to access the file directly to a different location, so instead of the above code you would have to write the following in the footer.inc.php file.



```
<?php
&#xA0; &#xA0; defined(&apos;thefooter&apos;) or header(&apos;Location: http://www.location.com&apos;);
&#xA0; &#xA0; echo(&apos;Copyright to me in the year 2000&apos;);
?>
```


In normal case a redirection to an external site would be annoying to the visitor, but since this visitor is more interested in hacking the site than in reading the content, I think it&apos;s only fair to create such an redirection. We dont&apos; realy want someome like this on our sites.

For the file protection I use .htaccess in which I say to protect the file itself and every .inc file

&lt;Files ~ &quot;^.*\.([Hh][Tt]|[Ii][Nn][Cc])&quot;&gt;
Order allow,deny
Deny from all
Satisfy All
&lt;/Files&gt;

The .htaccess file should result an Error 403 if someone tries to access the files directly. If for some reason this shouldn&apos;t work, then the &quot;Not with me my friend&quot; text apears or a redirection (depending what is used)

In my eyes this looks o.k. and safe.

  

#



If your PHP pages include() or require() files that live within the web server document root, for example library files in the same directory as the PHP pages, you must account for the possibility that attackers may call those library files directly.&#xA0; 

Any program level code in the library files (ie code not part of function definitions) will be directly executable by the caller outside of the scope of the intended calling sequence.&#xA0; An attacker may be able to leverage this ability to cause unintended effects.

The most robust way to guard against this possibility is to prevent your webserver from calling the library scripts directly, either by moving them out of the document root, or by putting them in a folder configured to refuse web server access. With Apache for example, create a .htaccess file in the library script folder with these directives:

Order Allow,Deny
Deny from any

  

#

[Official documentation page](https://www.php.net/manual/en/security.php)

**[To root](/README.md)**