# Mail Functions




<div class="phpcode"><span class="html">
Corrupted Attachments ???
<br>I spent many hours with corrupted attachments (of all types of files) - The answer: a blank line is needed after $msg.=$file \r\n \r\n [incredible but true].
<br>Heres some useful code for sending an attachment, and display html OR text depending on the users email-reader.
<br>
<br>i work with many different systems, so...
<br>
<br><span class="default">&lt;?php </span><span class="comment"># Is the OS Windows or Mac or Linux
<br></span><span class="keyword">if (</span><span class="default">strtoupper</span><span class="keyword">(</span><span class="default">substr</span><span class="keyword">(</span><span class="default">PHP_OS</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">,</span><span class="default">3</span><span class="keyword">)==</span><span class="string">&apos;WIN&apos;</span><span class="keyword">)) {
<br>&#xA0; </span><span class="default">$eol</span><span class="keyword">=</span><span class="string">&quot;\r\n&quot;</span><span class="keyword">;
<br>} elseif (</span><span class="default">strtoupper</span><span class="keyword">(</span><span class="default">substr</span><span class="keyword">(</span><span class="default">PHP_OS</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">,</span><span class="default">3</span><span class="keyword">)==</span><span class="string">&apos;MAC&apos;</span><span class="keyword">)) {
<br>&#xA0; </span><span class="default">$eol</span><span class="keyword">=</span><span class="string">&quot;\r&quot;</span><span class="keyword">; 
<br>} else {
<br>&#xA0; </span><span class="default">$eol</span><span class="keyword">=</span><span class="string">&quot;\n&quot;</span><span class="keyword">; 
<br>} </span><span class="default">?&gt;
<br></span>
<br><span class="default">&lt;?php
<br></span><span class="comment"># File for Attachment
<br></span><span class="default">$f_name</span><span class="keyword">=</span><span class="string">&quot;../../letters/&quot;</span><span class="keyword">.</span><span class="default">$letter</span><span class="keyword">;&#xA0; &#xA0; </span><span class="comment">// use relative path OR ELSE big headaches. $letter is my file for attaching.
<br></span><span class="default">$handle</span><span class="keyword">=</span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$f_name</span><span class="keyword">, </span><span class="string">&apos;rb&apos;</span><span class="keyword">);
<br></span><span class="default">$f_contents</span><span class="keyword">=</span><span class="default">fread</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">, </span><span class="default">filesize</span><span class="keyword">(</span><span class="default">$f_name</span><span class="keyword">));
<br></span><span class="default">$f_contents</span><span class="keyword">=</span><span class="default">chunk_split</span><span class="keyword">(</span><span class="default">base64_encode</span><span class="keyword">(</span><span class="default">$f_contents</span><span class="keyword">));&#xA0; &#xA0; </span><span class="comment">//Encode The Data For Transition using base64_encode();
<br></span><span class="default">$f_type</span><span class="keyword">=</span><span class="default">filetype</span><span class="keyword">(</span><span class="default">$f_name</span><span class="keyword">);
<br></span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">);
<br></span><span class="comment"># To Email Address
<br></span><span class="default">$emailaddress</span><span class="keyword">=</span><span class="string">&quot;user@example.com&quot;</span><span class="keyword">;
<br></span><span class="comment"># Message Subject
<br></span><span class="default">$emailsubject</span><span class="keyword">=</span><span class="string">&quot;Heres An Email with a PDF&quot;</span><span class="keyword">.</span><span class="default">date</span><span class="keyword">(</span><span class="string">&quot;Y/m/d H:i:s&quot;</span><span class="keyword">);
<br></span><span class="comment"># Message Body
<br></span><span class="default">ob_start</span><span class="keyword">();
<br>&#xA0; require(</span><span class="string">&quot;emailbody.php&quot;</span><span class="keyword">);&#xA0; &#xA0;&#xA0; </span><span class="comment">// i made a simple &amp; pretty page for showing in the email
<br></span><span class="default">$body</span><span class="keyword">=</span><span class="default">ob_get_contents</span><span class="keyword">(); </span><span class="default">ob_end_clean</span><span class="keyword">();
<br>
<br></span><span class="comment"># Common Headers
<br></span><span class="default">$headers </span><span class="keyword">.= </span><span class="string">&apos;From: Jonny &lt;jon@example.com&gt;&apos;</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">;
<br></span><span class="default">$headers </span><span class="keyword">.= </span><span class="string">&apos;Reply-To: Jonny &lt;jon@example.com&gt;&apos;</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">; 
<br></span><span class="default">$headers </span><span class="keyword">.= </span><span class="string">&apos;Return-Path: Jonny &lt;jon@example.com&gt;&apos;</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">;&#xA0; &#xA0;&#xA0; </span><span class="comment">// these two to set reply address
<br></span><span class="default">$headers </span><span class="keyword">.= </span><span class="string">&quot;Message-ID:&lt;&quot;</span><span class="keyword">.</span><span class="default">$now</span><span class="keyword">.</span><span class="string">&quot; TheSystem@&quot;</span><span class="keyword">.</span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;SERVER_NAME&apos;</span><span class="keyword">].</span><span class="string">&quot;&gt;&quot;</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">;
<br></span><span class="default">$headers </span><span class="keyword">.= </span><span class="string">&quot;X-Mailer: PHP v&quot;</span><span class="keyword">.</span><span class="default">phpversion</span><span class="keyword">().</span><span class="default">$eol</span><span class="keyword">;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// These two to help avoid spam-filters
<br># Boundry for marking the split &amp; Multitype Headers
<br></span><span class="default">$mime_boundary</span><span class="keyword">=</span><span class="default">md5</span><span class="keyword">(</span><span class="default">time</span><span class="keyword">());
<br></span><span class="default">$headers </span><span class="keyword">.= </span><span class="string">&apos;MIME-Version: 1.0&apos;</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">; 
<br></span><span class="default">$headers </span><span class="keyword">.= </span><span class="string">&quot;Content-Type: multipart/related; boundary=\&quot;&quot;</span><span class="keyword">.</span><span class="default">$mime_boundary</span><span class="keyword">.</span><span class="string">&quot;\&quot;&quot;</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">; 
<br></span><span class="default">$msg </span><span class="keyword">= </span><span class="string">&quot;&quot;</span><span class="keyword">;
<br>
<br></span><span class="comment"># Attachment
<br></span><span class="default">$msg </span><span class="keyword">.= </span><span class="string">&quot;--&quot;</span><span class="keyword">.</span><span class="default">$mime_boundary</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">;
<br></span><span class="default">$msg </span><span class="keyword">.= </span><span class="string">&quot;Content-Type: application/pdf; name=\&quot;&quot;</span><span class="keyword">.</span><span class="default">$letter</span><span class="keyword">.</span><span class="string">&quot;\&quot;&quot;</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">;&#xA0;&#xA0; </span><span class="comment">// sometimes i have to send MS Word, use &apos;msword&apos; instead of &apos;pdf&apos;
<br></span><span class="default">$msg </span><span class="keyword">.= </span><span class="string">&quot;Content-Transfer-Encoding: base64&quot;</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">;
<br></span><span class="default">$msg </span><span class="keyword">.= </span><span class="string">&quot;Content-Disposition: attachment; filename=\&quot;&quot;</span><span class="keyword">.</span><span class="default">$letter</span><span class="keyword">.</span><span class="string">&quot;\&quot;&quot;</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">; </span><span class="comment">// !! This line needs TWO end of lines !! IMPORTANT !!
<br></span><span class="default">$msg </span><span class="keyword">.= </span><span class="default">$f_contents</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">;
<br></span><span class="comment"># Setup for text OR html
<br></span><span class="default">$msg </span><span class="keyword">.= </span><span class="string">&quot;Content-Type: multipart/alternative&quot;</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">; 
<br>
<br></span><span class="comment"># Text Version
<br></span><span class="default">$msg </span><span class="keyword">.= </span><span class="string">&quot;--&quot;</span><span class="keyword">.</span><span class="default">$mime_boundary</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">;
<br></span><span class="default">$msg </span><span class="keyword">.= </span><span class="string">&quot;Content-Type: text/plain; charset=iso-8859-1&quot;</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">;
<br></span><span class="default">$msg </span><span class="keyword">.= </span><span class="string">&quot;Content-Transfer-Encoding: 8bit&quot;</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">;
<br></span><span class="default">$msg </span><span class="keyword">.= </span><span class="string">&quot;This is a multi-part message in MIME format.&quot;</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">;
<br></span><span class="default">$msg </span><span class="keyword">.= </span><span class="string">&quot;If you are reading this, please update your email-reading-software.&quot;</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">;
<br></span><span class="default">$msg </span><span class="keyword">.= </span><span class="string">&quot;+ + Text Only Email from Genius Jon + +&quot;</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">;
<br>
<br></span><span class="comment"># HTML Version
<br></span><span class="default">$msg </span><span class="keyword">.= </span><span class="string">&quot;--&quot;</span><span class="keyword">.</span><span class="default">$mime_boundary</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">;
<br></span><span class="default">$msg </span><span class="keyword">.= </span><span class="string">&quot;Content-Type: text/html; charset=iso-8859-1&quot;</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">;
<br></span><span class="default">$msg </span><span class="keyword">.= </span><span class="string">&quot;Content-Transfer-Encoding: 8bit&quot;</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">;
<br></span><span class="default">$msg </span><span class="keyword">.= </span><span class="default">$body</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">;
<br>
<br></span><span class="comment"># Finished
<br></span><span class="default">$msg </span><span class="keyword">.= </span><span class="string">&quot;--&quot;</span><span class="keyword">.</span><span class="default">$mime_boundary</span><span class="keyword">.</span><span class="string">&quot;--&quot;</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">.</span><span class="default">$eol</span><span class="keyword">;&#xA0;&#xA0; </span><span class="comment">// finish with two eol&apos;s for better security. see Injection.
<br>
<br># SEND THE EMAIL
<br></span><span class="default">ini_set</span><span class="keyword">(</span><span class="default">sendmail_from</span><span class="keyword">,</span><span class="string">&apos;from@example.com&apos;</span><span class="keyword">);&#xA0; </span><span class="comment">// the INI lines are to force the From Address to be used !
<br>&#xA0; </span><span class="default">mail</span><span class="keyword">(</span><span class="default">$emailaddress</span><span class="keyword">, </span><span class="default">$emailsubject</span><span class="keyword">, </span><span class="default">$msg</span><span class="keyword">, </span><span class="default">$headers</span><span class="keyword">); 
<br></span><span class="default">ini_restore</span><span class="keyword">(</span><span class="default">sendmail_from</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>
<br>I hope this helps.
<br>Jon Webb [Madrid&amp;London]
<br>
<br>[EDITOR thiago NOTE: Code has a fix from o.straesser AT gmx DOT de]</span>
</div>
  

#


<div class="phpcode"><span class="html">
As mentioned earlier, for Windows users there is a fake sendmail option. A bit more detailed description how to do this is:<br><br>If you have a test server in use running Windows and some kind of WAMP combo (XXAMP, WAMP Server, etc) then you&apos;ll notice that the PHP sendmail command (mail()) does not work. Windows simply does not provide the sendmail statement ...<br><br>There is a simple trick to get this to work though;<br><br>1) Download (or use the attached file) sendmail.zip from <a href="http://glob.com.au/sendmail/" rel="nofollow" target="_blank">http://glob.com.au/sendmail/</a><br><br>2) Unzip this in a folder on your c: drive (preferably use a simple path, for example c:\wamp\sendmail -- long filenames could cause problems)<br><br>3) Edit your PHP.INI file (note: WAMP users should access their php.ini file from the WAMP menu). Go to the [mail function] section and modify it as such:<br><br>[mail function]<br>; For Win32 only.<br>;SMTP =<br><br>; For Win32 only.<br>;sendmail_from =<br><br>; For Unix only.&#xA0; You may supply arguments as well (default: &quot;sendmail -t -i&quot;).<br>sendmail_path = &quot;C:\wamp\sendmail\sendmail.exe -t&quot;<br><br>; Force the addition of the specified parameters to be passed as extra parameters<br>; to the sendmail binary. These parameters will always replace the value of<br>; the 5th parameter to mail(), even in safe mode.<br>;mail.force_extra_paramaters =<br><br>.. and save the changes.<br><br>4) Open the sendmail.ini and modify the settings to:<br><br>[sendmail]<br><br>; you must change mail.mydomain.com to your smtp server,<br>; or to IIS&apos;s &quot;pickup&quot; directory.&#xA0; (generally C:\Inetpub\mailroot\Pickup)<br>; emails delivered via IIS&apos;s pickup directory cause sendmail to<br>; run quicker, but you won&apos;t get error messages back to the calling<br>; application.<br><br>smtp_server=mail.yourdomain.com<br><br>; smtp port (normally 25)<br><br>smtp_port=25<br><br>; the default domain for this server will be read from the registry<br>; this will be appended to email addresses when one isn&apos;t provided<br>; if you want to override the value in the registry, uncomment and modify<br><br>default_domain=yourdomain.com<br><br>; log smtp errors to error.log (defaults to same directory as sendmail.exe)<br>; uncomment to enable logging<br>; error_logfile=sendmail_error.log<br><br>; create debug log as debug.log (defaults to same directory as sendmail.exe)<br>; uncomment to enable debugging<br>; debug_logfile=sendmail_debug.log<br><br>; if your smtp server requires authentication, modify the following two lines<br><br>;auth_username=<br>;auth_password=<br><br>; if your smtp server uses pop3 before smtp authentication, modify the<br>; following three lines<br><br>pop3_server=mail.yourdomain.com<br>pop3_username=you@yourdomain.com<br>pop3_password=mysecretpassword<br><br>; to force the sender to always be the following email address, uncomment and<br>; populate with a valid email address.&#xA0; this will only affect the &quot;MAIL FROM&quot;<br>; command, it won&apos;t modify the &quot;From: &quot; header of the message content<br><br>force_sender=you@yourdomain.com<br><br>; sendmail will use your hostname and your default_domain in the ehlo/helo<br>; smtp greeting.&#xA0; you can manually set the ehlo/helo name if required<br><br>hostname=<br><br>The optional error and debug logging is recommended when trying this the first time, so you have a clue what goes wrong in case it doesn&apos;t work.<br>Force_sender is also optional, but recommended to avoid confusion on the server end.<br>Obviously mail.yourdomain.com, you@yourdomain.com, and mysecretpassword should be the relevant info for your SMTP server.<br>Now restart the WAMP services (mainly Apache so PHP re-reads it&apos;s config).<br><br>Now you&apos;re good to go and use the PHP mail() statement as if you&apos;re a Unix user ...</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/ref.mail.php)

**[⬆ to root](/)**