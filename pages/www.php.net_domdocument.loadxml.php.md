# DOMDocument::loadXML



Always remember that with the default parameters this function doesn&apos;t handle well large files, i.e. if a text node is longer than 10Mb it can raise an exception stating:<br><br>DOMDocument::loadXML(): internal error Extra content at the end of the document in Entity<br><br>even though the XML is fine.<br><br>The cause is a definition in parserInternals.h of lixml:<br>#define XML_MAX_TEXT_LENGTH 10000000<br><br>To allow the function to process larger files, pass the LIBXML_PARSEHUGE as an option and it will work just fine:<br><br>$domDocument-&gt;loadXML($xml, LIBXML_PARSEHUGE);  

#

loadXml reports an error instead of throwing an exception when the xml is not well formed. This is annoying if you are trying to to loadXml() in a try...catch statement. Apparently its a feature, not a bug, because this conforms to a spefication. <br><br>If you want to catch an exception instead of generating a report, you could do something like<br><br>

```
<?php
function HandleXmlError($errno, $errstr, $errfile, $errline)
{
    if ($errno==E_WARNING &amp;&amp; (substr_count($errstr,"DOMDocument::loadXML()")>0))
    {
        throw new DOMException($errstr);
    }
    else 
        return false;
}

function XmlLoader($strXml)
{
    set_error_handler('HandleXmlError');
    $dom = new DOMDocument();
    $dom->loadXml($strXml);    
    restore_error_handler();
    return $dom;
 }

?>
```
<br><br>Returning false in function HandleXmlError() causes a fallback to the default error handler.  

#

[Official documentation page](https://www.php.net/manual/en/domdocument.loadxml.php)

**[To root](/README.md)**