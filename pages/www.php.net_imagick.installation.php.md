# Installation





I struggled to get imagick installed for mamp and dug high and low. I
was left with the impression that so many struggle with opensource and
 this type of install that it is almost a right of passage. For my part, I
 managed to find two threads that helped me and the steps work. A lot
 of people talked about macports and pear installs which are fine(I tried
 them), but 1) they sometimes don&apos;t get the latest version of what
 you need and 2) I can&apos;t tell what they&apos;re doing and where they&apos;re doing it.

It&apos;s much easier to me to download the stuff where i want it and be 
sure of what i got!

First of all download a tar image of the ImageMagick install from here:
sourceforge.net/projects/imagemagick/files/

Unpack it and then from terminal issue the following commands in
 quotes:
1.&#xA0; &quot;cd ImageMagick-6.5.7&quot; - go where you placed the folder

2.&#xA0; &quot;./configure&quot;

3.&#xA0; &quot;make&quot;

If ImageMagick configured and compiled without complaint, you are
 ready to install it on your system. Administrator privileges are
 required to install. To install, type the following command in terminal:

1.&#xA0; &quot;sudo make install&quot;

To check your ImageMagick installation enter the following command
 in terminal:

1. &quot;make check&quot;

Remember all commands are entered into terminal in the directory in
 which you unzipped your downloaded ImageMagick tar file. (the latest
 version of which at this time of writing is ImageMagick-6.5.7.)

Next we need to install Imagick.so which is what we want for PHP.

First of all we need to get the right file and we can get that from here:

pecl.php.net/package/imagick/download

At current writing the latest stable version is imagick-2.3.0.

Unpack the tar file and then enter the commands in quotes in terminal:

1.&#xA0; &quot;cd imagick-2.2.3&quot; - go where you placed the folder

2.&#xA0;&#xA0; &quot;phpize&quot;

3.&#xA0;&#xA0; &quot;./configure --with-imagick=/opt/local&quot;

4.&#xA0;&#xA0; &quot;make&quot;

5.&#xA0; &#xA0; &quot;make install&quot;

If you look at the bottom of the output, it will tell you where it has 
placed the imagick.so module. For me it was placed in:
 
/imagick-2.3.0/modules

which is where i had unpacked the downloaded imagick-2.3.0 file.

Then copy it into your MAMP PHP extensions folder. 

For me it was: 
/Applications/MAMP/bin/php5/lib/php/extensions/no-debug-non-zts
-20050922/

Then, for PHP to honor the the extension, add the following line to the
 extensions section of your php.ini file:

extension=imagick.so

You should now have a working imagick extension with all of the
 resources for php found here:
php.net/manual/en/book.imagick.php

I will not claim credit for anyone for whom this works, that goes to:

Imagemagick who posted instructions here:
www.imagemagick.org/script/install-source.php

and Wallance who posted his experience here:
www.daniweb.com/forums/thread194181.html#

I really hope this helps others because developing this stuff is 
frustrating enough without spending hours or even days just trying to set
 up the development environment so you can use the tools.

  

#



If you&apos;re on a Ubuntu or debian machine, please don&apos;t dive into all those pecl stuff, but just use:

sudo apt-get install php7.1-imagick

where php7.1 is your php version, so could be different.

  

#

[Official documentation page](https://www.php.net/manual/en/imagick.installation.php)

**[To root](/README.md)**