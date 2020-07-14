# DOMDocument::__construct



The constuctor arguments are useful if you want to build a new document using createElement, appendChild etc.<br><br>By contrast, these arguments are overriden as soon as you load a document from source by calling load() or loadXML().<br><br>* If the source contains an XML declaration specifying an encoding, that encoding is used.<br>* If the XML declaration does not specify an encoding, or if the source does not contain a declaration at all, UTF-8 is assumed.<br><br>This behaviour applies no matter what you declared when you called new DOMDocument().  

#

[Official documentation page](https://www.php.net/manual/en/domdocument.construct.php)

**[To root](/README.md)**