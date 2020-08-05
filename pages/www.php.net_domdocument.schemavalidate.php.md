# DOMDocument::schemaValidate



For more detailed feedback from DOMDocument::schemaValidate, disable libxml errors and fetch error information yourself.  See http://php.net/manual/en/ref.libxml.php for more info.<br><br>example.xml<br>``<?xml version="1.0"?>``;<br>&lt;example&gt;<br>    &lt;child_string&gt;This is an example.&lt;/child_string&gt;<br>    &lt;child_integer&gt;Error condition.&lt;/child_integer&gt;<br>&lt;/example&gt;<br><br>example.xsd<br>``<?xml version="1.0"?>``;<br>&lt;xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"<br>elementFormDefault="qualified"&gt;<br>    &lt;xs:element name="example"&gt;<br>        &lt;xs:complexType&gt;<br>            &lt;xs:sequence&gt;<br>                &lt;xs:element name="child_string" type="xs:string"/&gt;<br>                &lt;xs:element name="child_integer" type="xs:integer"/&gt;<br>            &lt;/xs:sequence&gt;<br>        &lt;/xs:complexType&gt;<br>    &lt;/xs:element&gt;<br>&lt;/xs:schema&gt;<br><br>

```
<?php

function libxml_display_error($error)
{
    $return = "<br/>\n";
    switch ($error->level) {
        case LIBXML_ERR_WARNING:
            $return .= "<b>Warning $error->code</b>: ";
            break;
        case LIBXML_ERR_ERROR:
            $return .= "<b>Error $error->code</b>: ";
            break;
        case LIBXML_ERR_FATAL:
            $return .= "<b>Fatal Error $error->code</b>: ";
            break;
    }
    $return .= trim($error->message);
    if ($error->file) {
        $return .=    " in <b>$error->file</b>";
    }
    $return .= " on line <b>$error->line</b>\n";

    return $return;
}

function libxml_display_errors() {
    $errors = libxml_get_errors();
    foreach ($errors as $error) {
        print libxml_display_error($error);
    }
    libxml_clear_errors();
}

// Enable user error handling
libxml_use_internal_errors(true);

$xml = new DOMDocument(); 
$xml->load('example.xml'); 

if (!$xml->schemaValidate('example.xsd')) {
    print '<b>DOMDocument::schemaValidate() Generated Errors!</b>';
    libxml_display_errors();
}

?>
```
<br><br>Old error message:<br>Warning: DOMDocument::schemaValidate() [function.schemaValidate]: Element &apos;child_integer&apos;: &apos;Error condition.&apos; is not a valid value of the atomic type &apos;xs:integer&apos;. in example.php on line 40<br><br>New error message:<br>DOMDocument::schemaValidate() Generated Errors!<br>Error 1824: Element &apos;child_integer&apos;: &apos;Error condition.&apos; is not a valid value of the atomic type &apos;xs:integer&apos;. in example.xml on line 4  

---

[Official documentation page](https://www.php.net/manual/en/domdocument.schemavalidate.php)

**[To root](/README.md)**