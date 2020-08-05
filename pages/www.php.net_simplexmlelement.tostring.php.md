# SimpleXMLElement::__toString



[Someone remove that other Patanjali&apos;s note, because it has errors! :-(]<br><br>For those for whom it may not be immediately obvious from the example, the echo is what is forcing __toString() to be used.<br><br>However, to assign the text of a node (but not its children) to a variable:<br><br>$XML = new SimpleXMLElement(&apos;&lt;p&gt;Hello&lt;span&gt; world&lt;/span&gt;.&lt;span&gt; Good day!&lt;/span&gt;&lt;/p&gt;&apos;);<br><br>$Text = $XML-&gt;__toString();<br><br>is effectively:<br>$Text = &apos;Hello.&apos;; // The &lt;span&gt;s are ignored.<br><br>Either of:<br>$Text = $XML-&gt;span-&gt;__toString();<br>$Text = $XML-&gt;span[0]-&gt;__toString();<br><br>is effectively:<br>$Text = &apos; world&apos;; // Only the first &lt;span&gt; is used.<br><br>$Text = $XML-&gt;span[1]-&gt;__toString();<br><br>is effectively:<br>$Text = &apos; Good day!&apos;; // Only the second &lt;span&gt; is used.  

---

[Official documentation page](https://www.php.net/manual/en/simplexmlelement.tostring.php)

**[To root](/README.md)**