# Installation



because I confused in step 2 I write this comment:<br>for installing following this instruction step by step:<br>download PDFlib-Lite-X.X.XpX form first post...<br><br>open terminal and type: <br><br>$ cd download folder...<br>$ sudo tar -xzvf PDFlib-Lite-X.X.XpX.tar.gz <br># cd PDFlib-Lite-X.X.XpX <br># ./configure<br># make<br># make install <br><br>next:<br># apt-get install php5-dev    : online installation if not installed<br># apt-get install ?>
```
pear    : online installation if not installed<br># pecl install pdflib         : online installation<br>    The ask be: &apos;path to pdflib installation? : &apos; enter &apos;/usr/local&apos;<br><br># gedit /etc/php5/apache2/php.ini<br>    add &apos;extension=pdf.so in&apos; 1 line<br><br># /etc/init.d/apache2 reload  OR /etc/init.d/apache2 restart<br><br>Should work! for test you can write "$p = new PDF_new();" in a file.  

#

[Official documentation page](https://www.php.net/manual/en/pdf.installation.php)

**[To root](/README.md)**