# Document Object Model



If you need simple interface to DOM check out phpQuery - jQuery port to PHP:<br>http://code.google.com/p/phpquery/<br><br>It uses CSS selectors to fetch nodes.<br>Here&apos;s example how it works:<br>

```
<?php
// just one file to include
require('phpQuery/phpQuery.php');

$html = '
<div>
    mydiv
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </ul>
</div>';

// intialize new DOM from markup
phpQuery::newDocument($markup)
    ->find('ul > li')
        ->addClass('my-new-class')
        ->filter(':last')
            ->addClass('last-li');

// query all unordered lists in last used DOM
pq('ul')->insertAfter('div');

// iterate all LIs from last used DOM
foreach(pq('li') as $li) {
    // iteration returns plain DOM nodes, not phpQuery objects
    pq($li)->addClass('my-second-new-class');
}

// same as pq('anything')->htmlOuter()
// but on document root (returns doctype etc)
print phpQuery::getDocument();
?>
```
<br><br>It uses DOM extension and XPath so it works only in PHP5.  

#

[Official documentation page](https://www.php.net/manual/en/book.dom.php)

**[To root](/README.md)**