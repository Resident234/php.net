# SimpleXMLElement::children



Here&apos;s a simple, recursive, function to transform XML data into pseudo E4X syntax ie. root.child.value = foobar<br><br>

```
<?php
error_reporting(E_ALL);

$xml = new SimpleXMLElement(
'<Patriarch>
   <name>Bill</name>
   <wife>
     <name>Vi</name>
   </wife>
   <son>
     <name>Bill</name>
   </son>
   <daughter>
     <name>Jeri</name>
     <husband>
       <name>Mark</name>
     </husband>
     <son>
       <name>Greg</name>
     </son>
     <son>
       <name>Tim</name>
     </son>     
     <son>
       <name>Mark</name>
     </son>     
     <son>
       <name>Josh</name>
         <wife>
           <name>Kristine</name>
         </wife> 
         <son>
           <name>Blake</name>
         </son>
         <daughter>
           <name>Liah</name>
         </daughter>
     </son>
   </daughter>
</Patriarch>');

RecurseXML($xml);

function RecurseXML($xml,$parent="")
{
   $child_count = 0;
   foreach($xml as $key=>$value)
   {
      $child_count++;     
      if(RecurseXML($value,$parent.".".$key) == 0)  // no childern, aka "leaf node"
      {
         print($parent . "." . (string)$key . " = " . (string)$value . "<BR>\n");        
      }     
   }
   return $child_count;
}

?>
```
<br><br>The output....<br><br>.name = Bill<br>.wife.name = Vi<br>.son.name = Bill<br>.daughter.name = Jeri<br>.daughter.husband.name = Mark<br>.daughter.son.name = Greg<br>.daughter.son.name = Tim<br>.daughter.son.name = Mark<br>.daughter.son.name = Josh<br>.daughter.son.wife.name = Kristine<br>.daughter.son.son.name = Blake<br>.daughter.son.daughter.name = Liah  

---

[Official documentation page](https://www.php.net/manual/en/simplexmlelement.children.php)

**[To root](/README.md)**