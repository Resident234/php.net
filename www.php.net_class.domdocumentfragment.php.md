# The DOMDocumentFragment class




<div class="phpcode"><span class="html">
DOMDocumentFragment only appears useful if created from a parent DOMDocument eg.<br><br>1. $dom = new DOMDocument(&quot;1.0&quot;,&quot;UTF-8&quot;);<br>2. $docFrag = $dom-&gt;createDocumentFragment();<br>3. Now append items to $docFrag <br>4. Graft $docFrag contents back onto $dom at the desired location<br><br>Conversely taking this approach:<br>1. $dom = new DOMDocument(&quot;1.0&quot;,&quot;UTF-8&quot;);<br>2. $docFrag = new DOMDocumentFragment();<br>3. Now append items to $docFrag<br><br>...will fail on step 3 with a &quot;read only&quot; error as $docFrag is not created as a child of&#xA0; DOMDocument.<br><br>I&apos;m not sure of the reason for this: on the web people have cited security, and others have cited poor design however whatever the reason, it is really limiting when wanting to encapsulating generic independent DocumentFragments into classes for easy grafting to the desired tree. The only workarounds i have seen look expensive from a performance perspective and cumbersome from a coding perspective ie. create a&#xA0; dummy $dom for temporary use.<br><br>(This is valid as of PHP 5.3) I&apos;ve put this here as i&apos;ve wasted a lot of time finding it out - I hope this saves others some heartache.<br><br>Using new DOMDocumentFramt</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.domdocumentfragment.php)

**[⬆ to root](/)**