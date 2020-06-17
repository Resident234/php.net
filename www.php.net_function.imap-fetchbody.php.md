# imap_fetchbody




<div class="phpcode"><span class="html">
imap-fetchbody() will decode attached email messages inline with the rest of the email parts, however the way it works when handling attached email messages is inconsistent with the main email message.<br><br>With an email message that only has a text body and does not have any mime attachments, imap-fetchbody() will return the following for each requested part number:<br><br>(empty) - Entire message<br>0 - Message header<br>1 - Body text<br><br>With an email message that is a multi-part message in MIME format, and contains the message text in plain text and HTML, and has a file.ext attachment, imap-fetchbody() will return something like the following for each requested part number:<br><br>(empty) - Entire message<br>0 - Message header<br>1 - MULTIPART/ALTERNATIVE<br>1.1 - TEXT/PLAIN<br>1.2 - TEXT/HTML<br>2 - file.ext<br><br>Now if you attach the above email to an email with the message text in plain text and HTML, imap_fetchbody() will use this type of part number system:<br><br>(empty) - Entire message<br>0 - Message header<br>1 - MULTIPART/ALTERNATIVE<br>1.1 - TEXT/PLAIN<br>1.2 - TEXT/HTML<br>2 - MESSAGE/RFC822 (entire attached message)<br>2.0 - Attached message header<br>2.1 - TEXT/PLAIN<br>2.2 - TEXT/HTML<br>2.3 - file.ext<br><br>Note that the file.ext is on the same level now as the plain text and HTML, and that there is no way to access the MULTIPART/ALTERNATIVE in the attached message.<br><br>Here is a modified version of some of the code from previous posts that will build an easily accessible array that includes accessible attached message parts and the message body if there aren&apos;t multipart mimes.&#xA0; The $structure variable is the output of the imap_fetchstructure() function.&#xA0; The returned $part_array has the field &apos;part_number&apos; which contains the part number to be fed directly into the imap_fetchbody() function.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">create_part_array</span><span class="keyword">(</span><span class="default">$structure</span><span class="keyword">, </span><span class="default">$prefix</span><span class="keyword">=</span><span class="string">&quot;&quot;</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="comment">//print_r($structure);<br>&#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">sizeof</span><span class="keyword">(</span><span class="default">$structure</span><span class="keyword">-&gt;</span><span class="default">parts</span><span class="keyword">) &gt; </span><span class="default">0</span><span class="keyword">) {&#xA0; &#xA0; </span><span class="comment">// There some sub parts<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">foreach (</span><span class="default">$structure</span><span class="keyword">-&gt;</span><span class="default">parts </span><span class="keyword">as </span><span class="default">$count </span><span class="keyword">=&gt; </span><span class="default">$part</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">add_part_to_array</span><span class="keyword">(</span><span class="default">$part</span><span class="keyword">, </span><span class="default">$prefix</span><span class="keyword">.(</span><span class="default">$count</span><span class="keyword">+</span><span class="default">1</span><span class="keyword">), </span><span class="default">$part_array</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }else{&#xA0; &#xA0; </span><span class="comment">// Email does not have a seperate mime attachment for text<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$part_array</span><span class="keyword">[] = array(</span><span class="string">&apos;part_number&apos; </span><span class="keyword">=&gt; </span><span class="default">$prefix</span><span class="keyword">.</span><span class="string">&apos;1&apos;</span><span class="keyword">, </span><span class="string">&apos;part_object&apos; </span><span class="keyword">=&gt; </span><span class="default">$obj</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0;&#xA0; return </span><span class="default">$part_array</span><span class="keyword">;<br>}<br></span><span class="comment">// Sub function for create_part_array(). Only called by create_part_array() and itself. <br></span><span class="keyword">function </span><span class="default">add_part_to_array</span><span class="keyword">(</span><span class="default">$obj</span><span class="keyword">, </span><span class="default">$partno</span><span class="keyword">, &amp; </span><span class="default">$part_array</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">$part_array</span><span class="keyword">[] = array(</span><span class="string">&apos;part_number&apos; </span><span class="keyword">=&gt; </span><span class="default">$partno</span><span class="keyword">, </span><span class="string">&apos;part_object&apos; </span><span class="keyword">=&gt; </span><span class="default">$obj</span><span class="keyword">);<br>&#xA0; &#xA0; if (</span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">type </span><span class="keyword">== </span><span class="default">2</span><span class="keyword">) { </span><span class="comment">// Check to see if the part is an attached email message, as in the RFC-822 type<br>&#xA0; &#xA0; &#xA0; &#xA0; //print_r($obj);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">sizeof</span><span class="keyword">(</span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">parts</span><span class="keyword">) &gt; </span><span class="default">0</span><span class="keyword">) {&#xA0; &#xA0; </span><span class="comment">// Check to see if the email has parts<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">foreach (</span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">parts </span><span class="keyword">as </span><span class="default">$count </span><span class="keyword">=&gt; </span><span class="default">$part</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Iterate here again to compensate for the broken way that imap_fetchbody() handles attachments<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">sizeof</span><span class="keyword">(</span><span class="default">$part</span><span class="keyword">-&gt;</span><span class="default">parts</span><span class="keyword">) &gt; </span><span class="default">0</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$part</span><span class="keyword">-&gt;</span><span class="default">parts </span><span class="keyword">as </span><span class="default">$count2 </span><span class="keyword">=&gt; </span><span class="default">$part2</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">add_part_to_array</span><span class="keyword">(</span><span class="default">$part2</span><span class="keyword">, </span><span class="default">$partno</span><span class="keyword">.</span><span class="string">&quot;.&quot;</span><span class="keyword">.(</span><span class="default">$count2</span><span class="keyword">+</span><span class="default">1</span><span class="keyword">), </span><span class="default">$part_array</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }else{&#xA0; &#xA0; </span><span class="comment">// Attached email does not have a seperate mime attachment for text<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$part_array</span><span class="keyword">[] = array(</span><span class="string">&apos;part_number&apos; </span><span class="keyword">=&gt; </span><span class="default">$partno</span><span class="keyword">.</span><span class="string">&apos;.&apos;</span><span class="keyword">.(</span><span class="default">$count</span><span class="keyword">+</span><span class="default">1</span><span class="keyword">), </span><span class="string">&apos;part_object&apos; </span><span class="keyword">=&gt; </span><span class="default">$obj</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; }else{&#xA0; &#xA0; </span><span class="comment">// Not sure if this is possible<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$part_array</span><span class="keyword">[] = array(</span><span class="string">&apos;part_number&apos; </span><span class="keyword">=&gt; </span><span class="default">$prefix</span><span class="keyword">.</span><span class="string">&apos;.1&apos;</span><span class="keyword">, </span><span class="string">&apos;part_object&apos; </span><span class="keyword">=&gt; </span><span class="default">$obj</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }else{&#xA0; &#xA0; </span><span class="comment">// If there are more sub-parts, expand them out.<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">sizeof</span><span class="keyword">(</span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">parts</span><span class="keyword">) &gt; </span><span class="default">0</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">parts </span><span class="keyword">as </span><span class="default">$count </span><span class="keyword">=&gt; </span><span class="default">$p</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">add_part_to_array</span><span class="keyword">(</span><span class="default">$p</span><span class="keyword">, </span><span class="default">$partno</span><span class="keyword">.</span><span class="string">&quot;.&quot;</span><span class="keyword">.(</span><span class="default">$count</span><span class="keyword">+</span><span class="default">1</span><span class="keyword">), </span><span class="default">$part_array</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If text has been encoded as quoted-printable (most body text is encoded as this), it must be decoded for it to be displayed correctly (without &apos;=&apos;, &apos;=20&apos; and other strange text chunks all through the string).<br><br>To decode, you can use the following built-in php function...<br><br>quoted_printable_decode($string)<br><br>Hopefully I&apos;ve just saved a few people from having to do a preg_replace on there email bodies.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-fetchbody.php)

**[To root](/README.md)**