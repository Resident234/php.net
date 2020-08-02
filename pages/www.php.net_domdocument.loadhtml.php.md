# DOMDocument::loadHTML



You can also load HTML as UTF-8 using this simple hack:<br><br>

```
<?php

$doc = new DOMDocument();
$doc->loadHTML('

```
<?phpxml encoding="UTF-8">' . $html);

// dirty fix
foreach ($doc->childNodes as $item)
    if ($item->nodeType == XML_PI_NODE)
        $doc->removeChild($item); // remove hack
$doc->encoding = 'UTF-8'; // insert proper

?>
```
  

---

DOMDocument is very good at dealing with imperfect markup, but it throws warnings all over the place when it does. <br><br>This isn&apos;t well documented here. The solution to this is to implement a separate aparatus for dealing with just these errors. <br><br>Set libxml_use_internal_errors(true) before calling loadHTML. This will prevent errors from bubbling up to your default error handler. And you can then get at them (if you desire) using other libxml error functions. <br><br>You can find more info here http://www.php.net/manual/en/ref.libxml.php  

---

When using loadHTML() to process UTF-8 pages, you may meet the problem that the output of dom functions are not like the input. For example, if you want to get "C&#x1EA1;nh tranh", you will receive "C&#xE1;&#xBA;&#xA1;nh tranh".  I suggest we use mb_convert_encoding before load UTF-8 page :<br>

```
<?php
    $pageDom = new DomDocument();    
    $searchPage = mb_convert_encoding($htmlUTF8Page, 'HTML-ENTITIES', "UTF-8"); 
    @$pageDom->loadHTML($searchPage);

?>
```
  

---

Pay attention when loading html that has a different charset than iso-8859-1. Since this method does not actively try to figure out what the html you are trying to load is encoded in (like most browsers do), you have to specify it in the html head. If, for instance, your html is in utf-8, make sure you have a meta tag in the html&apos;s head section:<br><br>&lt;head&gt;<br>&lt;meta http-equiv="Content-Type" content="text/html; charset=utf-8"/&gt;<br>&lt;/head&gt;<br><br>If you do not specify the charset like this, all high-ascii bytes will be html-encoded. It is not enough to set the dom document you are loading the html in to UTF-8.  

---

[Official documentation page](https://www.php.net/manual/en/domdocument.loadhtml.php)

**[To root](/README.md)**