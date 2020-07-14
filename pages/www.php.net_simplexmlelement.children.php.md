# SimpleXMLElement::children



Here&apos;s a simple, recursive, function to transform XML data into pseudo E4X syntax ie. root.child.value = foobar<br><br>

```
<?php
error_reporting(E_ALL);

$xml = new SimpleXMLElement(
&apos;&lt;Patriarch&gt;
   &lt;name&gt;Bill&lt;/name&gt;
   &lt;wife&gt;
     &lt;name&gt;Vi&lt;/name&gt;
   &lt;/wife&gt;
   &lt;son&gt;
     &lt;name&gt;Bill&lt;/name&gt;
   &lt;/son&gt;
   &lt;daughter&gt;
     &lt;name&gt;Jeri&lt;/name&gt;
     &lt;husband&gt;
       &lt;name&gt;Mark&lt;/name&gt;
     &lt;/husband&gt;
     &lt;son&gt;
       &lt;name&gt;Greg&lt;/name&gt;
     &lt;/son&gt;
     &lt;son&gt;
       &lt;name&gt;Tim&lt;/name&gt;
     &lt;/son&gt;     
     &lt;son&gt;
       &lt;name&gt;Mark&lt;/name&gt;
     &lt;/son&gt;     
     &lt;son&gt;
       &lt;name&gt;Josh&lt;/name&gt;
         &lt;wife&gt;
           &lt;name&gt;Kristine&lt;/name&gt;
         &lt;/wife&gt; 
         &lt;son&gt;
           &lt;name&gt;Blake&lt;/name&gt;
         &lt;/son&gt;
         &lt;daughter&gt;
           &lt;name&gt;Liah&lt;/name&gt;
         &lt;/daughter&gt;
     &lt;/son&gt;
   &lt;/daughter&gt;
&lt;/Patriarch&gt;&apos;);

RecurseXML($xml);

function RecurseXML($xml,$parent="")
{
   $child_count = 0;
   foreach($xml as $key=&gt;$value)
   {
      $child_count++;     
      if(RecurseXML($value,$parent.".".$key) == 0)  // no childern, aka "leaf node"
      {
         print($parent . "." . (string)$key . " = " . (string)$value . "&lt;BR&gt;\n");        
      }     
   }
   return $child_count;
}

?>
```
<br><br>The output....<br><br>.name = Bill<br>.wife.name = Vi<br>.son.name = Bill<br>.daughter.name = Jeri<br>.daughter.husband.name = Mark<br>.daughter.son.name = Greg<br>.daughter.son.name = Tim<br>.daughter.son.name = Mark<br>.daughter.son.name = Josh<br>.daughter.son.wife.name = Kristine<br>.daughter.son.son.name = Blake<br>.daughter.son.daughter.name = Liah  

#

[Official documentation page](https://www.php.net/manual/en/simplexmlelement.children.php)

**[To root](/README.md)**