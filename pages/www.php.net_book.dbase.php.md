# dBase



Unfortunately the dbase functions are not compiled into my commercial server&apos;s php and I needed to read some geo data in shape files, which include data in dbfs.<br><br>So maybe this will help some others:<br><br>

```
<?php
function echo_dbf($dbfname) {
    $fdbf = fopen($dbfname,'r'); 
    $fields = array();
    $buf = fread($fdbf,32);
    $header=unpack( "VRecordCount/vFirstRecord/vRecordLength", substr($buf,4,8));
    echo 'Header: '.json_encode($header).'<br/>';
    $goon = true; 
    $unpackString='';
    while ($goon &amp;&amp; !feof($fdbf)) { // read fields:
        $buf = fread($fdbf,32);
        if (substr($buf,0,1)==chr(13)) {$goon=false;} // end of field list
        else {
            $field=unpack( "a11fieldname/A1fieldtype/Voffset/Cfieldlen/Cfielddec", substr($buf,0,18));
            echo 'Field: '.json_encode($field).'<br/>';
            $unpackString.="A$field[fieldlen]$field[fieldname]/";
            array_push($fields, $field);}}
    fseek($fdbf, $header['FirstRecord']+1); // move back to the start of the first record (after the field definitions)
    for ($i=1; $i<=$header['RecordCount']; $i++) {
        $buf = fread($fdbf,$header['RecordLength']);
        $record=unpack($unpackString,$buf);
        echo 'record: '.json_encode($record).'<br/>';
        echo $i.$buf.'<br/>';} //raw record
    fclose($fdbf); }
?>
```
<br><br>This function simply dumps an entire file using echo and json_encode, so you can tweak it to your own needs... (eg random access would just be a matter of changing the seek to : fseek($fdbf, $header[&apos;FirstRecord&apos;]+1 +($header[&apos;RecordLength&apos;]* $desiredrecord0based); removing the for loop and returning $record<br><br>This function doesn&apos;t do any type conversion, but it does extract the type if you need to play with dates, or tidy up the numbers etc.<br><br>So quick and dirty but maybe of use to somebody and illustrates the power of unpack.<br><br>Erich  

---

[Official documentation page](https://www.php.net/manual/en/book.dbase.php)

**[To root](/README.md)**