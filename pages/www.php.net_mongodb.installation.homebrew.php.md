# Installing the MongoDB PHP Driver on macOS with Homebrew





How to install on macOS Mojave

Start with:

sudo pecl install mongodb

To check if mongodb package is installed, look for &quot;mongodb&quot; when you run:

pecl list

To get your installed mongodb.so path, run:

pecl list-files mongodb | grep mongodb.so

then remove (or comment out) on your php.ini file:

extension=&quot;mongodb.so&quot; 

(I could not found a line with extension=&quot;php_mongodb.so&quot;)

now insert a line with:

extension=&quot;{{the path to your installed mongodb.so}}&quot;

Run this command to get your ext-*.ini directory path:

php -i | grep Scan

Create your ext-mongodb.ini with:

touch {{your conf.d path}}/ext-mongodb.ini

like: 
touch /usr/local/etc/php/7.1/conf.d/ext-mongodb.ini

Restart your apache to read the new configuration

Sanity check with:

php -i | grep mongodb 

Your&apos;re ready to go.

  

#



The mongodb install has been removed from Homebrew. To install the mongodb extension you need to use pecl.

sudo pecl install mongodb

You may need to follow some additional configuration. Follow the inline instructions as they appear. Once the install is finished it will add two lines at the bottom of your php.ini file. Instead, remove these lines and add the and add a separate file to your conf.d directory in the same directory as you php.ini file.

In php.ini remove

extension=&quot;mongodb.so&quot; // remove
extension=&quot;php_mongodb.so // remove 

Then run:

$ touch /usr/local/etc/php/5.6/conf.d/ext-mongodb.ini

Finally add this line to the new file:

// example
extension=&quot;/usr/local/Cellar/php@5.6/5.6.35/pecl/20131226/mongodb.so&quot;

  

#

[Official documentation page](https://www.php.net/manual/en/mongodb.installation.homebrew.php)

**[To root](/README.md)**