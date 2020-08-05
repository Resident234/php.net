# The DOMDocument class



Showing a quick example of how to use this class, just so that new users can get a quick start without having to figure it all out by themself. ( At the day of posting, this documentation just got added and is lacking examples. )<br><br>

```
<?php

// Set the content type to be XML, so that the browser will   recognise it as XML.
header( "content-type: application/xml; charset=ISO-8859-15" );

// "Create" the document.
$xml = new DOMDocument( "1.0", "ISO-8859-15" );

// Create some elements.
$xml_album = $xml->createElement( "Album" );
$xml_track = $xml->createElement( "Track", "The ninth symphony" );

// Set the attributes.
$xml_track->setAttribute( "length", "0:01:15" );
$xml_track->setAttribute( "bitrate", "64kb/s" );
$xml_track->setAttribute( "channels", "2" );

// Create another element, just to show you can add any (realistic to computer) number of sublevels.
$xml_note = $xml->createElement( "Note", "The last symphony composed by Ludwig van Beethoven." );

// Append the whole bunch.
$xml_track->appendChild( $xml_note );
$xml_album->appendChild( $xml_track );

// Repeat the above with some different values..
$xml_track = $xml->createElement( "Track", "Highway Blues" );

$xml_track->setAttribute( "length", "0:01:33" );
$xml_track->setAttribute( "bitrate", "64kb/s" );
$xml_track->setAttribute( "channels", "2" );
$xml_album->appendChild( $xml_track );

$xml->appendChild( $xml_album );

// Parse the XML.
print $xml->saveXML();

?>
```


Output:
<Album>
  <Track length="0:01:15" bitrate="64kb/s" channels="2">
    The ninth symphony
    <Note>
      The last symphony composed by Ludwig van Beethoven.
    </Note>
  </Track>
  <Track length="0:01:33" bitrate="64kb/s" channels="2">Highway Blues</Track>
</Album>

If you want your PHP->DOM code to run under the .xml extension, you should set your webserver up to run the .xml extension with PHP ( Refer to the installation/configuration configuration for PHP on how to do this ).

Note that this:


```
<?php
$xml = new DOMDocument( "1.0", "ISO-8859-15" );
$xml_album = $xml->createElement( "Album" );
$xml_track = $xml->createElement( "Track" );
$xml_album->appendChild( $xml_track );
$xml->appendChild( $xml_album );
?>
```


is NOT the same as this:


```
<?php
// Will NOT work.
$xml = new DOMDocument( "1.0", "ISO-8859-15" );
$xml_album = new DOMElement( "Album" );
$xml_track = new DOMElement( "Track" );
$xml_album->appendChild( $xml_track );
$xml->appendChild( $xml_album );
?>
```


although this will work:


```
<?php
$xml = new DOMDocument( "1.0", "ISO-8859-15" );
$xml_album = new DOMElement( "Album" );
$xml->appendChild( $xml_album );
?>
```
  

---

For those landing here and checking for encoding issue with utf-8 characteres, it&apos;s pretty easy to correct it, without adding any additional output tag to your html.<br><br>We&apos;ll be utilizing: mb_convert_encoding<br><br>Thanks to the user who shared: SmartDOMDocument in previous comments, I got the idea of solving it. However I truly wish that he shared the method instead of giving a link.<br><br>Anyway coming back to the solution, you can simply use:<br><br>

```
<?php

            // checks if the content we're receiving isn't empty, to avoid the warning
            if ( empty( $content ) ) {
                return false;
            }

            // converts all special characters to utf-8
            $content = mb_convert_encoding($content, 'HTML-ENTITIES', 'UTF-8');

            // creating new document
            $doc = new DOMDocument('1.0', 'utf-8');

            //turning off some errors
            libxml_use_internal_errors(true);

            // it loads the content without adding enclosing html/body tags and also the doctype declaration
            $doc->LoadHTML($content, LIBXML_HTML_NOIMPLIED | LIBXML_HTML_NODEFDTD);

            // do whatever you want to do with this code now

?>
```
<br><br>I hope it solves the issue for someone! If you need my help or service to fix your code, you can reach me on nabtron.com or contact me at the email mentioned with this comment.  

---

Here&apos;s a small function I wrote to get all page links using the DOMDocument which will hopefully be of use to others<br><br>

```
<?php
/**
 * @author Jay Gilford
 */
 
/**
 * get_links()
 * 
 * @param string $url
 * @return array
 */
function get_links($url) {
 
    // Create a new DOM Document to hold our webpage structure
    $xml = new DOMDocument();
 
    // Load the url's contents into the DOM
    $xml->loadHTMLFile($url);
 
    // Empty array to hold all links to return
    $links = array();
 
    //Loop through each <a> tag in the dom and add it to the link array
    foreach($xml->getElementsByTagName('a') as $link) {
        $links[] = array('url' => $link->getAttribute('href'), 'text' => $link->nodeValue);
    }
 
    //Return the links
    return $links;
}
?>
```
  

---

For anyone else who has been having issues with formatOuput not working, here is a work-around:<br><br>rather than just doing something like:<br><br>

```
<?php
$outXML = $xml->saveXML();
?>
```


force it to reload the XML from scratch, then it will format correctly:



```
<?php
$outXML = $xml->saveXML();
$xml = new DOMDocument();
$xml->preserveWhiteSpace = false;
$xml->formatOutput = true;
$xml->loadXML($outXML);
$outXML = $xml->saveXML();
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/class.domdocument.php)

**[To root](/README.md)**