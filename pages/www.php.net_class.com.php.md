# The com class



Took me a while to figure this out by trial and error:<br><br>If your frustrated when getting a com-exception like "Error [0x80004002] ..." (with a message nagging you about an unsupported interface), and you&apos;re calling a class method through COM that expects a char as an argument, you should NOT call the method with a single-character string.<br><br>The correct way of calling a method like this is to use a VARIANT with a type of VT_UI1.<br><br>COM enabled method definition: <br><br>public bool DoSomething(char arg);<br><br>

```
<?php

$com = new COM(&apos;Some.Class.Name&apos;);

// This will fail
$var = &apos;a&apos;;
$com-&gt;DoSomething($var);

// This works correctly
$var = new VARIANT(ord(&apos;a&apos;), VT_UI1);
$com-&gt;DoSomething($var);

?>
```
<br><br>I hope this helps some people  

#

After one week of trying to understand what was wrong with my PHP communication with MS Word, i finally got it working...<br>It seems that if you&apos;re running IIS, the COM object are invoked with restricted privileges.<br>If you&apos;re having permission problems like not being able to opening or saving a document and you&apos;re getting errors like:<br><br> - This command is not available because no document is open<br>or<br> - Command failed<br><br>try this (if you&apos;re running IIS):<br><br>- Execute "dcomcnfg"<br>- Open Component Services &gt; Computers &gt; My Computer &gt; DCOM Config<br>- Search for Microsoft Office Word 97-2003 Document (it will be something like this translated to your language, so take a while and search for it)<br>- Right-Click on it and open the properties<br>- Choose "Identity" tab<br>- Normally this is set to "the launching user". You have to change this to "the interactive user" or a admin user of your choice.<br>- Apply these new settings and test your COM application. It should work fine now.<br><br>I hope I save lots and lots of hours of headaches to some of you :)  

#

[Official documentation page](https://www.php.net/manual/en/class.com.php)

**[To root](/README.md)**