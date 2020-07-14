# DOMDocument::schemaValidate



For more detailed feedback from DOMDocument::schemaValidate, disable libxml errors and fetch error information yourself.  See http://php.net/manual/en/ref.libxml.php for more info.<br><br>example.xml<br>&lt;?xml version="1.0"?>
```
<br>&lt;example&gt;<br>    &lt;child_string&gt;This is an example.&lt;/child_string&gt;<br>    &lt;child_integer&gt;Error condition.&lt;/child_integer&gt;<br>&lt;/example&gt;<br><br>example.xsd<br>&lt;?xml version="1.0"?>
```
<br>&lt;xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"<br>elementFormDefault="qualified"&gt;<br>    &lt;xs:element name="example"&gt;<br>        &lt;xs:complexType&gt;<br>            &lt;xs:sequence&gt;<br>                &lt;xs:element name="child_string" type="xs:string"/&gt;<br>                &lt;xs:element name="child_integer" type="xs:integer"/&gt;<br>            &lt;/xs:sequence&gt;<br>        &lt;/xs:complexType&gt;<br>    &lt;/xs:element&gt;<br>&lt;/xs:schema&gt;<br><br>

```
<?php

function libxml_display_error($error)
{
    $return = "&lt;br/&gt;\n";
    switch ($error-&gt;level) {
        case LIBXML_ERR_WARNING:
            $return .= "&lt;b&gt;Warning $error-&gt;code&lt;/b&gt;: ";
            break;
        case LIBXML_ERR_ERROR:
            $return .= "&lt;b&gt;Error $error-&gt;code&lt;/b&gt;: ";
            break;
        case LIBXML_ERR_FATAL:
            $return .= "&lt;b&gt;Fatal Error $error-&gt;code&lt;/b&gt;: ";
            break;
    }
    $return .= trim($error-&gt;message);
    if ($error-&gt;file) {
        $return .=    " in &lt;b&gt;$error-&gt;file&lt;/b&gt;";
    }
    $return .= " on line &lt;b&gt;$error-&gt;line&lt;/b&gt;\n";

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
$xml-&gt;load(&apos;example.xml&apos;); 

if (!$xml-&gt;schemaValidate(&apos;example.xsd&apos;)) {
    print &apos;&lt;b&gt;DOMDocument::schemaValidate() Generated Errors!&lt;/b&gt;&apos;;
    libxml_display_errors();
}

?>
```
<br><br>Old error message:<br>Warning: DOMDocument::schemaValidate() [function.schemaValidate]: Element &apos;child_integer&apos;: &apos;Error condition.&apos; is not a valid value of the atomic type &apos;xs:integer&apos;. in example.php on line 40<br><br>New error message:<br>DOMDocument::schemaValidate() Generated Errors!<br>Error 1824: Element &apos;child_integer&apos;: &apos;Error condition.&apos; is not a valid value of the atomic type &apos;xs:integer&apos;. in example.xml on line 4  

#

[Official documentation page](https://www.php.net/manual/en/domdocument.schemavalidate.php)

**[To root](/README.md)**