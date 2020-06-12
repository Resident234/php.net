# Dealing with XML errors




<div class="phpcode"><span class="html">
Note that &quot;if (! $sxe) {&quot; may give you a false-negative if the XML document was empty (e.g. &quot;&lt;root /&gt;&quot;).&#xA0; In that case, $sxe will be:<br><br>object(SimpleXMLElement)#1 (0) {<br>}<br><br>which will evaluate to false, even though nothing technically went wrong.<br><br>Consider instead: &quot;if ($sxe === false) {&quot;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/simplexml.examples-errors.php)

**[⬆ to root](/)**