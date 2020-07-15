# Examples



When using simplexml to access a element the returned object may be a SimpleXMLElement instead of a string.<br><br>Example:<br><br>

```
<?php
$string = &lt;&lt;&lt;XML
&lt;?xml version='1.0'?>
```

&lt;document&gt;
    &lt;cmd&gt;login&lt;/cmd&gt;
    &lt;login&gt;Richard&lt;/login&gt;
&lt;/document&gt;
XML;
                                                                        
                                           
$xml = simplexml_load_string($string);
print_r($xml);
$login = $xml->login;
print_r($login);
$login = (string) $xml->login;
print_r($login);
?>
```
<br><br>Expected result:<br>----------------<br>SimpleXMLElement Object<br>(<br>    [cmd] =&gt; login<br>    [login] =&gt; Richard<br>)<br>Richard<br>Richard<br><br>Actual result:<br>--------------<br>SimpleXMLElement Object<br>(<br>    [cmd] =&gt; login<br>    [login] =&gt; Richard<br>)<br>SimpleXMLElement Object<br>(<br>    [0] =&gt; Richard<br>)<br>Richard<br><br>But this is an intended behavior. See http://bugs.php.net/bug.php?id=29500  

#

[Official documentation page](https://www.php.net/manual/en/simplexml.examples.php)

**[To root](/README.md)**