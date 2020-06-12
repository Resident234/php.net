# imap_append




<div class="phpcode"><span class="html">
Here is how I used this function with an attachment. I used examples from everyone else but made it easier and cleaner to change.
<br>
<br><span class="default">&lt;?php
<br>$authhost</span><span class="keyword">=</span><span class="string">&quot;{000.000.000.000:993/validate-cert/ssl}Drafts&quot;</span><span class="keyword">;
<br>
<br></span><span class="default">$user</span><span class="keyword">=</span><span class="string">&quot;sadasd&quot;</span><span class="keyword">;
<br></span><span class="default">$pass</span><span class="keyword">=</span><span class="string">&quot;sadasd&quot;</span><span class="keyword">;
<br>
<br>if (</span><span class="default">$mbox</span><span class="keyword">=</span><span class="default">imap_open</span><span class="keyword">( </span><span class="default">$authhost</span><span class="keyword">, </span><span class="default">$user</span><span class="keyword">, </span><span class="default">$pass</span><span class="keyword">))
<br>{
<br>&#xA0; &#xA0; </span><span class="default">$dmy</span><span class="keyword">=</span><span class="default">date</span><span class="keyword">(</span><span class="string">&quot;d-M-Y H:i:s&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="default">$filename</span><span class="keyword">=</span><span class="string">&quot;filename.pdf&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$attachment </span><span class="keyword">= </span><span class="default">chunk_split</span><span class="keyword">(</span><span class="default">base64_encode</span><span class="keyword">(</span><span class="default">$filestring</span><span class="keyword">));
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="default">$boundary </span><span class="keyword">= </span><span class="string">&quot;------=&quot;</span><span class="keyword">.</span><span class="default">md5</span><span class="keyword">(</span><span class="default">uniqid</span><span class="keyword">(</span><span class="default">rand</span><span class="keyword">()));
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="default">$msg </span><span class="keyword">= (</span><span class="string">&quot;From: Somebody\r\n&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;To: test@example.co.uk\r\n&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;Date: </span><span class="default">$dmy</span><span class="string">\r\n&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;Subject: This is the subject\r\n&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;MIME-Version: 1.0\r\n&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;Content-Type: multipart/mixed; boundary=\&quot;</span><span class="default">$boundary</span><span class="string">\&quot;\r\n&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;\r\n\r\n&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;--</span><span class="default">$boundary</span><span class="string">\r\n&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;Content-Type: text/html;\r\n\tcharset=\&quot;ISO-8859-1\&quot;\r\n&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;Content-Transfer-Encoding: 8bit \r\n&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;\r\n\r\n&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;Hello this is a test\r\n&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;\r\n\r\n&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;--</span><span class="default">$boundary</span><span class="string">\r\n&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;Content-Transfer-Encoding: base64\r\n&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;Content-Disposition: attachment; filename=\&quot;</span><span class="default">$filename</span><span class="string">\&quot;\r\n&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;\r\n&quot; </span><span class="keyword">. </span><span class="default">$attachment </span><span class="keyword">. </span><span class="string">&quot;\r\n&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;\r\n\r\n\r\n&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">. </span><span class="string">&quot;--</span><span class="default">$boundary</span><span class="string">--\r\n\r\n&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="default">imap_append</span><span class="keyword">(</span><span class="default">$mbox</span><span class="keyword">,</span><span class="default">$authhost</span><span class="keyword">,</span><span class="default">$msg</span><span class="keyword">, </span><span class="string">&quot;\\Draft&quot;</span><span class="keyword">); 
<br>
<br>&#xA0; &#xA0; </span><span class="default">imap_close</span><span class="keyword">(</span><span class="default">$mbox</span><span class="keyword">);
<br>}
<br>else
<br>{
<br>&#xA0; &#xA0; echo </span><span class="string">&quot;&lt;h1&gt;FAIL!&lt;/h1&gt;\n&quot;</span><span class="keyword">;
<br>}
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>Hope this helps someone.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Hi,<br><br>As we have been struggling with this for some time I wanted to share how we got imap_append working properly with all MIME parts including attachments.&#xA0; If you are sending email and also wish to append the sent message to the Sent Items folder, I cannot think of an easier way to do this, as follows:<br><br>1) Use SwiftMailer to send the message via PHP.<br>$message = Swift_Message::newInstance(&quot;Subject goes here&quot;);<br>(then add from, to, body, attachments etc)<br>$result = $mailer-&gt;send($message);<br><br>2) When you construct the message in step 1) above save it to a variable as follows:<br><br>$msg = $message-&gt;toString(); (this creates the full MIME message required for imap_append()!!&#xA0; After this you can call imap_append like this:<br><br>imap_append($imap_conn,$mail_box,$msg.&quot;\r\n&quot;,&quot;\\Seen&quot;);<br><br>I hope this helps the readers, and prevents saves people from doing what we started doing - hand crafting the MIME messages :-0</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-append.php)

**[⬆ to root](/)**