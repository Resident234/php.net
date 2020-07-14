# The SimpleXMLIterator class



The documentation is a bit sparse for SimpleXmlIterator.  Here is an example showing the use of its methods. xml2Array and sxiToArray work together to convert an XML document to an associative array structure.<br><br>The contents of cats.xml:<br>======================================<br>&lt;cats&gt;<br>  &lt;cat&gt;<br>      &lt;name&gt;Jack&lt;/name&gt;<br>      &lt;age&gt;2&lt;/age&gt;<br>      &lt;color&gt;grey&lt;/color&gt;<br>      &lt;color&gt;white&lt;/color&gt;<br>  &lt;/cat&gt;<br>  &lt;cat&gt;<br>      &lt;name&gt;Maxwell&lt;/name&gt;<br>      &lt;age&gt;12&lt;/age&gt;<br>      &lt;color&gt;orange&lt;/color&gt;<br>      &lt;color&gt;black&lt;/color&gt;<br>  &lt;/cat&gt;<br>&lt;/cats&gt;<br>======================================<br><br>

```
<?php
function xml2array($fname){
  $sxi = new SimpleXmlIterator($fname, null, true);
  return sxiToArray($sxi);
}

function sxiToArray($sxi){
  $a = array();
  for( $sxi-&gt;rewind(); $sxi-&gt;valid(); $sxi-&gt;next() ) {
    if(!array_key_exists($sxi-&gt;key(), $a)){
      $a[$sxi-&gt;key()] = array();
    }
    if($sxi-&gt;hasChildren()){
      $a[$sxi-&gt;key()][] = sxiToArray($sxi-&gt;current());
    }
    else{
      $a[$sxi-&gt;key()][] = strval($sxi-&gt;current());
    }
  }
  return $a;
}

// Read cats.xml and print the results:
$catArray = xml2array(&apos;cats.xml&apos;);
print_r($catArray);
?>
```
<br><br>Results (reformatted a bit for compactness and clarity):<br>======================================<br>Array(<br>  [cat] =&gt; Array(<br>    [0] =&gt; Array(<br>      [name] =&gt; Array(  [0] =&gt; Jack    )<br>      [age] =&gt; Array(   [0] =&gt; 2       )<br>      [color] =&gt; Array( [0] =&gt; grey,<br>                        [1] =&gt; white   )<br>    )<br>    [1] =&gt; Array(<br>      [name] =&gt; Array(  [0] =&gt; Maxwell )<br>      [age] =&gt; Array(   [0] =&gt; 12      )<br>      [color] =&gt; Array( [0] =&gt; orange<br>                        [1] =&gt; black   )<br>    )<br>  )<br>)  

#

[Official documentation page](https://www.php.net/manual/en/class.simplexmliterator.php)

**[To root](/README.md)**