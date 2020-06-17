# Installation




<div class="phpcode"><span class="html">
because I confused in step 2 I write this comment:<br>for installing following this instruction step by step:<br>download PDFlib-Lite-X.X.XpX form first post...<br><br>open terminal and type: <br><br>$ cd download folder...<br>$ sudo tar -xzvf PDFlib-Lite-X.X.XpX.tar.gz <br># cd PDFlib-Lite-X.X.XpX <br># ./configure<br># make<br># make install <br><br>next:<br># apt-get install php5-dev&#xA0; &#xA0; : online installation if not installed<br># apt-get install php-pear&#xA0; &#xA0; : online installation if not installed<br># pecl install pdflib&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; : online installation<br>&#xA0; &#xA0; The ask be: &apos;path to pdflib installation? : &apos; enter &apos;/usr/local&apos;<br><br># gedit /etc/php5/apache2/php.ini<br>&#xA0; &#xA0; add &apos;extension=pdf.so in&apos; 1 line<br><br># /etc/init.d/apache2 reload&#xA0; OR /etc/init.d/apache2 restart<br><br>Should work! for test you can write &quot;$p = new PDF_new();&quot; in a file.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pdf.installation.php)

**[To root](/README.md)**