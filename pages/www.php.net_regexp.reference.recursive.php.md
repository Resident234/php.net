# Recursive patterns



With the (?R) item you can link only to the full pattern, because it quasi equals to (?0). You can not use anchors, asserts etc., and you can only check that string CONTAINS a valid hierarchy or not.<br><br>This is wrong: ^\(((?>
```
[^()]+)|(?R))*\)$<br><br>However, you can bracketing the full expression, and replace (?R) to the relative link (?-2). This make it reusable. So you can check complex expressions, for example:<br>

```
<?php

$bracket_system = "(\\(((?>
```
[^()]+)|(?-2))*\\))"; // (reuseable)
$bracket_systems = "((?>
```
[^()]+)?$bracket_system)*(?>
```
[^()]+)?"; // (reuseable)
$equation = "$bracket_systems=$bracket_systems"; // Both side of the equation must be contain valid bracket systems
var_dump(preg_match("/^$equation\$/","a*(a-(2a+2))=4(a+3)-2(a-(a-2))")); // Outputs 'int(1)'
var_dump(preg_match("/^$equation\$/","a*(a-(2a+2)=4(a+3)-2(a-(a-2)))")); // Outputs 'int(0)'

?>
```


You can also catch multibyte quotes with the 'u' modifier (if you use UTF-8), eg:


```
<?php

$quoted = "(&#xBB;((?>
```
[^&#xBB;&#xAB;]+)|(?-2))*&#xAB;)"; // (reuseable)
$prompt = "[\\w ]+: $quoted";
var_dump(preg_match("/^$prompt\$/u","Your name: &#xBB;write here&#xAB;")); // Outputs 'int(1)'

?>
```
  

#

The recursion in regular expressions is the only way to allow the parsing of HTML code with nested tags of indefinite depth.<br>It seems it&apos;s not yet a spreaded practice; not so much contents are available on the web regarding regexp recursion, and until now no user contribute notes have been published on this manual page.<br>I made several tests with complex patterns to get tags with specific attributes or namespaces, studying the recursion of a subpattern only instead of the full pattern.<br>Here&apos;s an example that may power a fast LL parser with recursive descent (http://en.wikipedia.org/wiki/Recursive_descent_parser):<br><br>$pattern = "/&lt;([\w]+)([^&gt;]*?) (([\s]*\/&gt;)| (&gt;((([^&lt;]*?|&lt;\!\-\-.*?\-\-&gt;)| (?R))*)&lt;\/\\1[\s]*&gt;))/xsm";<br><br>The performances of a preg_match or preg_match_all function call over an avarage (x)HTML document are quite fast and may drive you to chose this way instead of classic DOM object methods, which have a lot of limits and are usually poor in performance with their workarounds, too.<br>I post a sample application in a brief function (easy to be turned into OOP), which returns an array of objects:<br><br>

```
<?php
// test function:
function parse($html) {
    // I have split the pattern in two lines not to have long lines alerts by the PHP.net form:
    $pattern = "/&lt;([\w]+)([^&gt;]*?)(([\s]*\/&gt;)|".
    "(&gt;((([^&lt;]*?|&lt;\!\-\-.*?\-\->)|(?R))*)&lt;\/\\1[\s]*&gt;))/sm";
    preg_match_all($pattern, $html, $matches, PREG_OFFSET_CAPTURE);
    $elements = array();
    
    foreach ($matches[0] as $key => $match) {
        $elements[] = (object)array(
            'node' => $match[0],
            'offset' => $match[1],
            'tagname' => $matches[1][$key][0],
            'attributes' => isset($matches[2][$key][0]) ? $matches[2][$key][0] : '',
            'omittag' => ($matches[4][$key][1] &gt; -1), // boolean
            'inner_html' => isset($matches[6][$key][0]) ? $matches[6][$key][0] : ''
        );
    }
    return $elements;
}

// random html nodes as example:
$html = &lt;&lt;&lt;EOD
&lt;div id="airport"&gt;
    &lt;div geo:position="1.234324,3.455546" class="index"&gt;
        &lt;!-- comment test:
        &lt;div class="index_top" /&gt;
        -->
        &lt;div class="element decorator"&gt;
                &lt;ul class="lister"&gt;
                    &lt;li onclick="javascript:item.showAttribute('desc');"&gt;
                        &lt;h3 class="outline"&gt;
                            &lt;a href="http://php.net/manual/en/regexp.reference.recursive.php" onclick="openPopup()"&gt;Link&lt;/a&gt;
                        &lt;/h3&gt;
                        &lt;div class="description"&gt;Sample description&lt;/div&gt;
                    &lt;/li&gt;
                &lt;/ul&gt;
        &lt;/div&gt;
        &lt;div class="clean-line"&gt;&lt;/div&gt;
    &lt;/div&gt;
&lt;/div&gt;
&lt;div id="omittag_test" rel="rootChild" /&gt;
EOD;

// application:
$elements = parse($html);

if (count($elements) &gt; 0) {
    echo "Elements found: &lt;b&gt;".count($elements)."&lt;/b&gt;&lt;br /&gt;";
    
    foreach ($elements as $element) {
        echo "&lt;p&gt;Tpl node: &lt;pre&gt;".htmlentities($element->node)."&lt;/pre&gt;
        Tagname: &lt;tt&gt;".$element->tagname."&lt;/tt&gt;&lt;br /&gt;
        Attributes: &lt;tt&gt;".$element->attributes."&lt;/tt&gt;&lt;br /&gt;
        Omittag: &lt;tt&gt;".($element->omittag ? 'true' : 'false')."&lt;/tt&gt;&lt;br /&gt;
        Inner HTML: &lt;pre&gt;".htmlentities($element->inner_html)."&lt;/pre&gt;&lt;/p&gt;";
    }
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/regexp.reference.recursive.php)

**[To root](/README.md)**