# htmlentities



An important note below about using this function to secure your application against Cross Site Scripting (XSS) vulnerabilities.<br><br>When printing user input in an attribute of an HTML tag, the default configuration of htmlEntities() doesn&apos;t protect you against XSS, when using single quotes to define the border of the tag&apos;s attribute-value. XSS is then possible by injecting a single quote:<br><br>

```
<?php
$_GET[&apos;a&apos;] = "#000&apos; onload=&apos;alert(document.cookie)";
?>
```


XSS possible (insecure):



```
<?php
$href = htmlEntities($_GET[&apos;a&apos;]);
print "&lt;body bgcolor=&apos;$href&apos;&gt;"; # results in: &lt;body bgcolor=&apos;#000&apos; onload=&apos;alert(document.cookie)&apos;&gt;
?>
```


Use the &apos;ENT_QUOTES&apos; quote style option, to ensure no XSS is possible and your application is secure:



```
<?php
$href = htmlEntities($_GET[&apos;a&apos;], ENT_QUOTES);
print "&lt;body bgcolor=&apos;$href&apos;&gt;"; # results in: &lt;body bgcolor=&apos;#000&amp;#039; onload=&amp;#039;alert(document.cookie)&apos;&gt;
?>
```


The &apos;ENT_QUOTES&apos; option doesn&apos;t protect you against javascript evaluation in certain tag&apos;s attributes, like the &apos;href&apos; attribute of the &apos;a&apos; tag. When clicked on the link below, the given JavaScript will get executed:



```
<?php
$_GET[&apos;a&apos;] = &apos;javascript:alert(document.cookie)&apos;;
$href = htmlEntities($_GET[&apos;a&apos;], ENT_QUOTES);
print "&lt;a href=&apos;$href&apos;&gt;link&lt;/a&gt;"; # results in: &lt;a href=&apos;javascript:alert(document.cookie)&apos;&gt;link&lt;/a&gt;
?>
```
  

#

I&apos;ve seen lots of functions to convert all the entities, but I needed to do a fulltext search in a db field that had named entities instead of numeric entities (edited by tinymce), so I searched the tinymce source and found a string with the value-&gt;entity mapping. So, i wrote the following function to encode the user&apos;s query with named entities.<br><br>The string I used is different of the original, because i didn&apos;t want to convert &apos; or ". The string is too long, so I had to cut it. To get the original check TinyMCE source and search for nbsp or other entity ;)<br><br>

```
<?php

$entities_unmatched = explode(&apos;,&apos;, &apos;160,nbsp,161,iexcl,162,cent, [...] &apos;);
$even = 1;
foreach($entities_unmatched as $c) {
    if($even) {
        $ord = $c;
    } else {
        $entities_table[$ord] = $c;
    }
    $even = 1 - $even;
}

function encode_named_entities($str) {
    global $entities_table;
    
    $encoded_str = &apos;&apos;;
    for($i = 0; $i &lt; strlen($str); $i++) {
        $ent = @$entities_table[ord($str{$i})];
        if($ent) {
            $encoded_str .= "&amp;$ent;";
        } else {
            $encoded_str .= $str{$i};
        }
    }
    return $encoded_str;
}

?>
```
  

#

html entities does not encode all unicode characters. It encodes what it can [all of latin1], and the others slip through. &amp;#1033; is the nasty I use. I have searched for a function which encodes everything, but in the end I wrote this. This is as simple as I can get it. Consult an ansii table to custom include/omit chars you want/don&apos;t. I&apos;m sure it&apos;s not that fast.<br><br>// Unicode-proof htmlentities. <br>// Returns &apos;normal&apos; chars as chars and weirdos as numeric html entites.<br>function superentities( $str ){<br>    // get rid of existing entities else double-escape<br>    $str = html_entity_decode(stripslashes($str),ENT_QUOTES,&apos;UTF-8&apos;); <br>    $ar = preg_split(&apos;/(?&lt;!^)(?!$)/u&apos;, $str );  // return array of every multi-byte character<br>    foreach ($ar as $c){<br>        $o = ord($c);<br>        if ( (strlen($c) &gt; 1) || /* multi-byte [unicode] */<br>            ($o &lt;32 || $o &gt; 126) || /* &lt;- control / latin weirdos -&gt; */<br>            ($o &gt;33 &amp;&amp; $o &lt; 40) ||/* quotes + ambersand */<br>            ($o &gt;59 &amp;&amp; $o &lt; 63) /* html */<br>        ) {<br>            // convert to numeric entity<br>            $c = mb_encode_numericentity($c,array (0x0, 0xffff, 0, 0xffff), &apos;UTF-8&apos;);<br>        }<br>        $str2 .= $c;<br>    }<br>    return $str2;<br>}  

#

If you are building a loadvars page for Flash and have problems with special chars such as " &amp; ", " &apos; " etc, you should escape them for flash:<br><br>Try trace(escape("&amp;")); in flash&apos; actionscript to see the escape code for &amp;;<br><br>% = %25<br>&amp; = %26<br>&apos; = %27<br><br>

```
<?php
function flashentities($string){
return str_replace(array("&amp;","&apos;"),array("%26","%27"),$string);
}
?>
```
<br><br>Those are the two that concerned me. YMMV.  

#

The following will make a string completely safe for XML:<br><br>

```
<?php
function philsXMLClean($strin) {
        $strout = null;

        for ($i = 0; $i &lt; strlen($strin); $i++) {
                $ord = ord($strin[$i]);

                if (($ord &gt; 0 &amp;&amp; $ord &lt; 32) || ($ord &gt;= 127)) {
                        $strout .= "&amp;amp;#{$ord};";
                }
                else {
                        switch ($strin[$i]) {
                                case &apos;&lt;&apos;:
                                        $strout .= &apos;&amp;lt;&apos;;
                                        break;
                                case &apos;&gt;&apos;:
                                        $strout .= &apos;&amp;gt;&apos;;
                                        break;
                                case &apos;&amp;&apos;:
                                        $strout .= &apos;&amp;amp;&apos;;
                                        break;
                                case &apos;"&apos;:
                                        $strout .= &apos;&amp;quot;&apos;;
                                        break;
                                default:
                                        $strout .= $strin[$i];
                        }
                }
        }

        return $strout;
}
?>
```
  

#

For those Spanish (and not only) folks, that want their national letters back after htmlentities :)<br><br>

```
<?php
protected function _decodeAccented($encodedValue, $options = array()) {
    $options += array(
        &apos;quote&apos;     =&gt; ENT_NOQUOTES,
        &apos;encoding&apos;  =&gt; &apos;UTF-8&apos;,
    );
    return preg_replace_callback(
        &apos;/&amp;\w(acute|uml|tilde);/&apos;,
        create_function(
            &apos;$m&apos;,
            &apos;return html_entity_decode($m[0], &apos; . $options[&apos;quote&apos;] . &apos;, "&apos; .
            $options[&apos;encoding&apos;] . &apos;");&apos;
        ),
        $encodedValue
    );
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.htmlentities.php)

**[To root](/README.md)**