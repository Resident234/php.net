# IMAP, POP3 and NNTP




<div class="phpcode"><span class="html">
For all the people coming here praying for:
<br>
<br>1) a dead-easy way to read MIME attachments, or
<br>2) a dead-easy way to access POP3 folders
<br>
<br>Look no further.
<br>
<br><span class="default">&lt;?php 
<br></span><span class="keyword">function </span><span class="default">pop3_login</span><span class="keyword">(</span><span class="default">$host</span><span class="keyword">,</span><span class="default">$port</span><span class="keyword">,</span><span class="default">$user</span><span class="keyword">,</span><span class="default">$pass</span><span class="keyword">,</span><span class="default">$folder</span><span class="keyword">=</span><span class="string">&quot;INBOX&quot;</span><span class="keyword">,</span><span class="default">$ssl</span><span class="keyword">=</span><span class="default">false</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; </span><span class="default">$ssl</span><span class="keyword">=(</span><span class="default">$ssl</span><span class="keyword">==</span><span class="default">false</span><span class="keyword">)?</span><span class="string">&quot;/novalidate-cert&quot;</span><span class="keyword">:</span><span class="string">&quot;&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; return (</span><span class="default">imap_open</span><span class="keyword">(</span><span class="string">&quot;{&quot;</span><span class="keyword">.</span><span class="string">&quot;</span><span class="default">$host</span><span class="string">:</span><span class="default">$port</span><span class="string">/pop3</span><span class="default">$ssl</span><span class="string">&quot;</span><span class="keyword">.</span><span class="string">&quot;}</span><span class="default">$folder</span><span class="string">&quot;</span><span class="keyword">,</span><span class="default">$user</span><span class="keyword">,</span><span class="default">$pass</span><span class="keyword">));
<br>}
<br>function </span><span class="default">pop3_stat</span><span class="keyword">(</span><span class="default">$connection</span><span class="keyword">)&#xA0; &#xA0; &#xA0; &#xA0; 
<br>{
<br>&#xA0; &#xA0; </span><span class="default">$check </span><span class="keyword">= </span><span class="default">imap_mailboxmsginfo</span><span class="keyword">(</span><span class="default">$connection</span><span class="keyword">);
<br>&#xA0; &#xA0; return ((array)</span><span class="default">$check</span><span class="keyword">);
<br>}
<br>function </span><span class="default">pop3_list</span><span class="keyword">(</span><span class="default">$connection</span><span class="keyword">,</span><span class="default">$message</span><span class="keyword">=</span><span class="string">&quot;&quot;</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; if (</span><span class="default">$message</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$range</span><span class="keyword">=</span><span class="default">$message</span><span class="keyword">;
<br>&#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$MC </span><span class="keyword">= </span><span class="default">imap_check</span><span class="keyword">(</span><span class="default">$connection</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$range </span><span class="keyword">= </span><span class="string">&quot;1:&quot;</span><span class="keyword">.</span><span class="default">$MC</span><span class="keyword">-&gt;</span><span class="default">Nmsgs</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; </span><span class="default">$response </span><span class="keyword">= </span><span class="default">imap_fetch_overview</span><span class="keyword">(</span><span class="default">$connection</span><span class="keyword">,</span><span class="default">$range</span><span class="keyword">);
<br>&#xA0; &#xA0; foreach (</span><span class="default">$response </span><span class="keyword">as </span><span class="default">$msg</span><span class="keyword">) </span><span class="default">$result</span><span class="keyword">[</span><span class="default">$msg</span><span class="keyword">-&gt;</span><span class="default">msgno</span><span class="keyword">]=(array)</span><span class="default">$msg</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$result</span><span class="keyword">;
<br>}
<br>function </span><span class="default">pop3_retr</span><span class="keyword">(</span><span class="default">$connection</span><span class="keyword">,</span><span class="default">$message</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; return(</span><span class="default">imap_fetchheader</span><span class="keyword">(</span><span class="default">$connection</span><span class="keyword">,</span><span class="default">$message</span><span class="keyword">,</span><span class="default">FT_PREFETCHTEXT</span><span class="keyword">));
<br>}
<br>function </span><span class="default">pop3_dele</span><span class="keyword">(</span><span class="default">$connection</span><span class="keyword">,</span><span class="default">$message</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; return(</span><span class="default">imap_delete</span><span class="keyword">(</span><span class="default">$connection</span><span class="keyword">,</span><span class="default">$message</span><span class="keyword">));
<br>}
<br>function </span><span class="default">mail_parse_headers</span><span class="keyword">(</span><span class="default">$headers</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; </span><span class="default">$headers</span><span class="keyword">=</span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/\r\n\s+/m&apos;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">,</span><span class="default">$headers</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">preg_match_all</span><span class="keyword">(</span><span class="string">&apos;/([^: ]+): (.+?(?:\r\n\s(?:.+?))*)?\r\n/m&apos;</span><span class="keyword">, </span><span class="default">$headers</span><span class="keyword">, </span><span class="default">$matches</span><span class="keyword">);
<br>&#xA0; &#xA0; foreach (</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] as </span><span class="default">$key </span><span class="keyword">=&gt;</span><span class="default">$value</span><span class="keyword">) </span><span class="default">$result</span><span class="keyword">[</span><span class="default">$value</span><span class="keyword">]=</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">][</span><span class="default">$key</span><span class="keyword">];
<br>&#xA0; &#xA0; return(</span><span class="default">$result</span><span class="keyword">);
<br>}
<br>function </span><span class="default">mail_mime_to_array</span><span class="keyword">(</span><span class="default">$imap</span><span class="keyword">,</span><span class="default">$mid</span><span class="keyword">,</span><span class="default">$parse_headers</span><span class="keyword">=</span><span class="default">false</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; </span><span class="default">$mail </span><span class="keyword">= </span><span class="default">imap_fetchstructure</span><span class="keyword">(</span><span class="default">$imap</span><span class="keyword">,</span><span class="default">$mid</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$mail </span><span class="keyword">= </span><span class="default">mail_get_parts</span><span class="keyword">(</span><span class="default">$imap</span><span class="keyword">,</span><span class="default">$mid</span><span class="keyword">,</span><span class="default">$mail</span><span class="keyword">,</span><span class="default">0</span><span class="keyword">);
<br>&#xA0; &#xA0; if (</span><span class="default">$parse_headers</span><span class="keyword">) </span><span class="default">$mail</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">][</span><span class="string">&quot;parsed&quot;</span><span class="keyword">]=</span><span class="default">mail_parse_headers</span><span class="keyword">(</span><span class="default">$mail</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">][</span><span class="string">&quot;data&quot;</span><span class="keyword">]);
<br>&#xA0; &#xA0; return(</span><span class="default">$mail</span><span class="keyword">);
<br>}
<br>function </span><span class="default">mail_get_parts</span><span class="keyword">(</span><span class="default">$imap</span><span class="keyword">,</span><span class="default">$mid</span><span class="keyword">,</span><span class="default">$part</span><span class="keyword">,</span><span class="default">$prefix</span><span class="keyword">)
<br>{&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="default">$attachments</span><span class="keyword">=array();
<br>&#xA0; &#xA0; </span><span class="default">$attachments</span><span class="keyword">[</span><span class="default">$prefix</span><span class="keyword">]=</span><span class="default">mail_decode_part</span><span class="keyword">(</span><span class="default">$imap</span><span class="keyword">,</span><span class="default">$mid</span><span class="keyword">,</span><span class="default">$part</span><span class="keyword">,</span><span class="default">$prefix</span><span class="keyword">);
<br>&#xA0; &#xA0; if (isset(</span><span class="default">$part</span><span class="keyword">-&gt;</span><span class="default">parts</span><span class="keyword">)) </span><span class="comment">// multipart
<br>&#xA0; &#xA0; </span><span class="keyword">{
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$prefix </span><span class="keyword">= (</span><span class="default">$prefix </span><span class="keyword">== </span><span class="string">&quot;0&quot;</span><span class="keyword">)?</span><span class="string">&quot;&quot;</span><span class="keyword">:</span><span class="string">&quot;</span><span class="default">$prefix</span><span class="string">.&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$part</span><span class="keyword">-&gt;</span><span class="default">parts </span><span class="keyword">as </span><span class="default">$number</span><span class="keyword">=&gt;</span><span class="default">$subpart</span><span class="keyword">) 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$attachments</span><span class="keyword">=</span><span class="default">array_merge</span><span class="keyword">(</span><span class="default">$attachments</span><span class="keyword">, </span><span class="default">mail_get_parts</span><span class="keyword">(</span><span class="default">$imap</span><span class="keyword">,</span><span class="default">$mid</span><span class="keyword">,</span><span class="default">$subpart</span><span class="keyword">,</span><span class="default">$prefix</span><span class="keyword">.(</span><span class="default">$number</span><span class="keyword">+</span><span class="default">1</span><span class="keyword">)));
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return </span><span class="default">$attachments</span><span class="keyword">;
<br>}
<br>function </span><span class="default">mail_decode_part</span><span class="keyword">(</span><span class="default">$connection</span><span class="keyword">,</span><span class="default">$message_number</span><span class="keyword">,</span><span class="default">$part</span><span class="keyword">,</span><span class="default">$prefix</span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; </span><span class="default">$attachment </span><span class="keyword">= array();
<br>
<br>&#xA0; &#xA0; if(</span><span class="default">$part</span><span class="keyword">-&gt;</span><span class="default">ifdparameters</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$part</span><span class="keyword">-&gt;</span><span class="default">dparameters </span><span class="keyword">as </span><span class="default">$object</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$attachment</span><span class="keyword">[</span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$object</span><span class="keyword">-&gt;</span><span class="default">attribute</span><span class="keyword">)]=</span><span class="default">$object</span><span class="keyword">-&gt;</span><span class="default">value</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$object</span><span class="keyword">-&gt;</span><span class="default">attribute</span><span class="keyword">) == </span><span class="string">&apos;filename&apos;</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$attachment</span><span class="keyword">[</span><span class="string">&apos;is_attachment&apos;</span><span class="keyword">] = </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$attachment</span><span class="keyword">[</span><span class="string">&apos;filename&apos;</span><span class="keyword">] = </span><span class="default">$object</span><span class="keyword">-&gt;</span><span class="default">value</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; if(</span><span class="default">$part</span><span class="keyword">-&gt;</span><span class="default">ifparameters</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$part</span><span class="keyword">-&gt;</span><span class="default">parameters </span><span class="keyword">as </span><span class="default">$object</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$attachment</span><span class="keyword">[</span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$object</span><span class="keyword">-&gt;</span><span class="default">attribute</span><span class="keyword">)]=</span><span class="default">$object</span><span class="keyword">-&gt;</span><span class="default">value</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if(</span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">$object</span><span class="keyword">-&gt;</span><span class="default">attribute</span><span class="keyword">) == </span><span class="string">&apos;name&apos;</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$attachment</span><span class="keyword">[</span><span class="string">&apos;is_attachment&apos;</span><span class="keyword">] = </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$attachment</span><span class="keyword">[</span><span class="string">&apos;name&apos;</span><span class="keyword">] = </span><span class="default">$object</span><span class="keyword">-&gt;</span><span class="default">value</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; </span><span class="default">$attachment</span><span class="keyword">[</span><span class="string">&apos;data&apos;</span><span class="keyword">] = </span><span class="default">imap_fetchbody</span><span class="keyword">(</span><span class="default">$connection</span><span class="keyword">, </span><span class="default">$message_number</span><span class="keyword">, </span><span class="default">$prefix</span><span class="keyword">);
<br>&#xA0; &#xA0; if(</span><span class="default">$part</span><span class="keyword">-&gt;</span><span class="default">encoding </span><span class="keyword">== </span><span class="default">3</span><span class="keyword">) { </span><span class="comment">// 3 = BASE64
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$attachment</span><span class="keyword">[</span><span class="string">&apos;data&apos;</span><span class="keyword">] = </span><span class="default">base64_decode</span><span class="keyword">(</span><span class="default">$attachment</span><span class="keyword">[</span><span class="string">&apos;data&apos;</span><span class="keyword">]);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; elseif(</span><span class="default">$part</span><span class="keyword">-&gt;</span><span class="default">encoding </span><span class="keyword">== </span><span class="default">4</span><span class="keyword">) { </span><span class="comment">// 4 = QUOTED-PRINTABLE
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$attachment</span><span class="keyword">[</span><span class="string">&apos;data&apos;</span><span class="keyword">] = </span><span class="default">quoted_printable_decode</span><span class="keyword">(</span><span class="default">$attachment</span><span class="keyword">[</span><span class="string">&apos;data&apos;</span><span class="keyword">]);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; return(</span><span class="default">$attachment</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>[EDIT BY danbrown AT php DOT net: Contains a bugfix by &quot;mn26826&quot; on 09-JUN-2010, which fixed the erroneous reference to $imap as the parameter passed to imap_mailboxmsginfo() within the user function pop3_stat().&#xA0; This was intended to be $connection.]
<br>
<br>[EDIT BY visualmind AT php DOT net: Contains a bugfix by &quot;elias-jobview&quot; on 17-AUG-2010, which fixed the error in pop3_list function which didn&apos;t have: return $result]
<br>
<br>[EDIT BY danbrown AT php DOT net: Contains a bugfix by &quot;chrismeistre&quot; on 09-SEP-2010, which fixed the erroneous reference to $mbox (should be $connection) in the pop3_list() function.]</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/book.imap.php)

**[⬆ to root](/)**