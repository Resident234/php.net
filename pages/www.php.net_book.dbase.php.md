# dBase





Unfortunately the dbase functions are not compiled into my commercial server&apos;s php and I needed to read some geo data in shape files, which include data in dbfs.



So maybe this will help some others:





```
<?php

function echo_dbf($dbfname) {

&#xA0; &#xA0; $fdbf = fopen($dbfname,&apos;r&apos;); 

&#xA0; &#xA0; $fields = array();

&#xA0; &#xA0; $buf = fread($fdbf,32);

&#xA0; &#xA0; $header=unpack( &quot;VRecordCount/vFirstRecord/vRecordLength&quot;, substr($buf,4,8));

&#xA0; &#xA0; echo &apos;Header: &apos;.json_encode($header).&apos;&lt;br/&gt;&apos;;

&#xA0; &#xA0; $goon = true; 

&#xA0; &#xA0; $unpackString=&apos;&apos;;

&#xA0; &#xA0; while ($goon &amp;&amp; !feof($fdbf)) { // read fields:

&#xA0; &#xA0; &#xA0; &#xA0; $buf = fread($fdbf,32);

&#xA0; &#xA0; &#xA0; &#xA0; if (substr($buf,0,1)==chr(13)) {$goon=false;} // end of field list

&#xA0; &#xA0; &#xA0; &#xA0; else {

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $field=unpack( &quot;a11fieldname/A1fieldtype/Voffset/Cfieldlen/Cfielddec&quot;, substr($buf,0,18));

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo &apos;Field: &apos;.json_encode($field).&apos;&lt;br/&gt;&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $unpackString.=&quot;A$field[fieldlen]$field[fieldname]/&quot;;

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; array_push($fields, $field);}}

&#xA0; &#xA0; fseek($fdbf, $header[&apos;FirstRecord&apos;]+1); // move back to the start of the first record (after the field definitions)

&#xA0; &#xA0; for ($i=1; $i&lt;=$header[&apos;RecordCount&apos;]; $i++) {

&#xA0; &#xA0; &#xA0; &#xA0; $buf = fread($fdbf,$header[&apos;RecordLength&apos;]);

&#xA0; &#xA0; &#xA0; &#xA0; $record=unpack($unpackString,$buf);

&#xA0; &#xA0; &#xA0; &#xA0; echo &apos;record: &apos;.json_encode($record).&apos;&lt;br/&gt;&apos;;

&#xA0; &#xA0; &#xA0; &#xA0; echo $i.$buf.&apos;&lt;br/&gt;&apos;;} //raw record

&#xA0; &#xA0; fclose($fdbf); }

?>
```




This function simply dumps an entire file using echo and json_encode, so you can tweak it to your own needs... (eg random access would just be a matter of changing the seek to : fseek($fdbf, $header[&apos;FirstRecord&apos;]+1 +($header[&apos;RecordLength&apos;]* $desiredrecord0based); removing the for loop and returning $record



This function doesn&apos;t do any type conversion, but it does extract the type if you need to play with dates, or tidy up the numbers etc.



So quick and dirty but maybe of use to somebody and illustrates the power of unpack.



Erich

  

#

[Official documentation page](https://www.php.net/manual/en/book.dbase.php)

**[To root](/README.md)**