# Installation





because I confused in step 2 I write this comment:
for installing following this instruction step by step:
download PDFlib-Lite-X.X.XpX form first post...

open terminal and type: 

$ cd download folder...
$ sudo tar -xzvf PDFlib-Lite-X.X.XpX.tar.gz 
# cd PDFlib-Lite-X.X.XpX 
# ./configure
# make
# make install 

next:
# apt-get install php5-dev&#xA0; &#xA0; : online installation if not installed
# apt-get install php-pear&#xA0; &#xA0; : online installation if not installed
# pecl install pdflib&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; : online installation
&#xA0; &#xA0; The ask be: &apos;path to pdflib installation? : &apos; enter &apos;/usr/local&apos;

# gedit /etc/php5/apache2/php.ini
&#xA0; &#xA0; add &apos;extension=pdf.so in&apos; 1 line

# /etc/init.d/apache2 reload&#xA0; OR /etc/init.d/apache2 restart

Should work! for test you can write &quot;$p = new PDF_new();&quot; in a file.

  

#

[Official documentation page](https://www.php.net/manual/en/pdf.installation.php)

**[To root](/README.md)**