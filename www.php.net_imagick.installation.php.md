# Installation




<div class="phpcode"><span class="html">
I struggled to get imagick installed for mamp and dug high and low. I<br>was left with the impression that so many struggle with opensource and<br> this type of install that it is almost a right of passage. For my part, I<br> managed to find two threads that helped me and the steps work. A lot<br> of people talked about macports and pear installs which are fine(I tried<br> them), but 1) they sometimes don&apos;t get the latest version of what<br> you need and 2) I can&apos;t tell what they&apos;re doing and where they&apos;re doing it.<br><br>It&apos;s much easier to me to download the stuff where i want it and be <br>sure of what i got!<br><br>First of all download a tar image of the ImageMagick install from here:<br>sourceforge.net/projects/imagemagick/files/<br><br>Unpack it and then from terminal issue the following commands in<br> quotes:<br>1.&#xA0; &quot;cd ImageMagick-6.5.7&quot; - go where you placed the folder<br><br>2.&#xA0; &quot;./configure&quot;<br><br>3.&#xA0; &quot;make&quot;<br><br>If ImageMagick configured and compiled without complaint, you are<br> ready to install it on your system. Administrator privileges are<br> required to install. To install, type the following command in terminal:<br><br>1.&#xA0; &quot;sudo make install&quot;<br><br>To check your ImageMagick installation enter the following command<br> in terminal:<br><br>1. &quot;make check&quot;<br><br>Remember all commands are entered into terminal in the directory in<br> which you unzipped your downloaded ImageMagick tar file. (the latest<br> version of which at this time of writing is ImageMagick-6.5.7.)<br><br>Next we need to install Imagick.so which is what we want for PHP.<br><br>First of all we need to get the right file and we can get that from here:<br><br>pecl.php.net/package/imagick/download<br><br>At current writing the latest stable version is imagick-2.3.0.<br><br>Unpack the tar file and then enter the commands in quotes in terminal:<br><br>1.&#xA0; &quot;cd imagick-2.2.3&quot; - go where you placed the folder<br><br>2.&#xA0;&#xA0; &quot;phpize&quot;<br><br>3.&#xA0;&#xA0; &quot;./configure --with-imagick=/opt/local&quot;<br><br>4.&#xA0;&#xA0; &quot;make&quot;<br><br>5.&#xA0; &#xA0; &quot;make install&quot;<br><br>If you look at the bottom of the output, it will tell you where it has <br>placed the imagick.so module. For me it was placed in:<br> <br>/imagick-2.3.0/modules<br><br>which is where i had unpacked the downloaded imagick-2.3.0 file.<br><br>Then copy it into your MAMP PHP extensions folder. <br><br>For me it was: <br>/Applications/MAMP/bin/php5/lib/php/extensions/no-debug-non-zts<br>-20050922/<br><br>Then, for PHP to honor the the extension, add the following line to the<br> extensions section of your php.ini file:<br><br>extension=imagick.so<br><br>You should now have a working imagick extension with all of the<br> resources for php found here:<br>php.net/manual/en/book.imagick.php<br><br>I will not claim credit for anyone for whom this works, that goes to:<br><br>Imagemagick who posted instructions here:<br>www.imagemagick.org/script/install-source.php<br><br>and Wallance who posted his experience here:<br>www.daniweb.com/forums/thread194181.html#<br><br>I really hope this helps others because developing this stuff is <br>frustrating enough without spending hours or even days just trying to set<br> up the development environment so you can use the tools.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you&apos;re on a Ubuntu or debian machine, please don&apos;t dive into all those pecl stuff, but just use:<br><br>sudo apt-get install php7.1-imagick<br><br>where php7.1 is your php version, so could be different.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/imagick.installation.php)

**[⬆ to root](/)**