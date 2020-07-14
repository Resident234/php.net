# Document Object Model



If you need simple interface to DOM check out phpQuery - jQuery port to PHP:<br>http://code.google.com/p/phpquery/<br><br>It uses CSS selectors to fetch nodes.<br>Here&apos;s example how it works:<br>

```
<?php
// just one file to include
require(&apos;phpQuery/phpQuery.php&apos;);

$html = &apos;
&lt;div&gt;
    mydiv
    &lt;ul&gt;
        &lt;li&gt;1&lt;/li&gt;
        &lt;li&gt;2&lt;/li&gt;
        &lt;li&gt;3&lt;/li&gt;
    &lt;/ul&gt;
&lt;/div&gt;&apos;;

// intialize new DOM from markup
phpQuery::newDocument($markup)
    -&gt;find(&apos;ul &gt; li&apos;)
        -&gt;addClass(&apos;my-new-class&apos;)
        -&gt;filter(&apos;:last&apos;)
            -&gt;addClass(&apos;last-li&apos;);

// query all unordered lists in last used DOM
pq(&apos;ul&apos;)-&gt;insertAfter(&apos;div&apos;);

// iterate all LIs from last used DOM
foreach(pq(&apos;li&apos;) as $li) {
    // iteration returns plain DOM nodes, not phpQuery objects
    pq($li)-&gt;addClass(&apos;my-second-new-class&apos;);
}

// same as pq(&apos;anything&apos;)-&gt;htmlOuter()
// but on document root (returns doctype etc)
print phpQuery::getDocument();
?>
```
<br><br>It uses DOM extension and XPath so it works only in PHP5.  

#

[Official documentation page](https://www.php.net/manual/en/book.dom.php)

**[To root](/README.md)**