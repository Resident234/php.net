# Error Messages Explained




<div class="phpcode"><span class="html">
Update to Adams old comment.<br><br>This is probably useful to someone.<br><br><span class="default">&lt;?php<br><br>$phpFileUploadErrors </span><span class="keyword">= array(<br>&#xA0; &#xA0; </span><span class="default">0 </span><span class="keyword">=&gt; </span><span class="string">&apos;There is no error, the file uploaded with success&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="default">1 </span><span class="keyword">=&gt; </span><span class="string">&apos;The uploaded file exceeds the upload_max_filesize directive in php.ini&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="default">2 </span><span class="keyword">=&gt; </span><span class="string">&apos;The uploaded file exceeds the MAX_FILE_SIZE directive that was specified in the HTML form&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="default">3 </span><span class="keyword">=&gt; </span><span class="string">&apos;The uploaded file was only partially uploaded&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="default">4 </span><span class="keyword">=&gt; </span><span class="string">&apos;No file was uploaded&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="default">6 </span><span class="keyword">=&gt; </span><span class="string">&apos;Missing a temporary folder&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="default">7 </span><span class="keyword">=&gt; </span><span class="string">&apos;Failed to write file to disk.&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; </span><span class="default">8 </span><span class="keyword">=&gt; </span><span class="string">&apos;A PHP extension stopped the file upload.&apos;</span><span class="keyword">,<br>);</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
[EDIT BY danbrown AT php DOT net: This code is a fixed version of a note originally submitted by (Thalent, Michiel Thalen) on 04-Mar-2009.]
<br>
<br>
<br>This is a handy exception to use when handling upload errors:
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">class </span><span class="default">UploadException </span><span class="keyword">extends </span><span class="default">Exception
<br></span><span class="keyword">{
<br>&#xA0; &#xA0; public function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$code</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$message </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">codeToMessage</span><span class="keyword">(</span><span class="default">$code</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">parent</span><span class="keyword">::</span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$message</span><span class="keyword">, </span><span class="default">$code</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; private function </span><span class="default">codeToMessage</span><span class="keyword">(</span><span class="default">$code</span><span class="keyword">)
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; switch (</span><span class="default">$code</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">UPLOAD_ERR_INI_SIZE</span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$message </span><span class="keyword">= </span><span class="string">&quot;The uploaded file exceeds the upload_max_filesize directive in php.ini&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">UPLOAD_ERR_FORM_SIZE</span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$message </span><span class="keyword">= </span><span class="string">&quot;The uploaded file exceeds the MAX_FILE_SIZE directive that was specified in the HTML form&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">UPLOAD_ERR_PARTIAL</span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$message </span><span class="keyword">= </span><span class="string">&quot;The uploaded file was only partially uploaded&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">UPLOAD_ERR_NO_FILE</span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$message </span><span class="keyword">= </span><span class="string">&quot;No file was uploaded&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">UPLOAD_ERR_NO_TMP_DIR</span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$message </span><span class="keyword">= </span><span class="string">&quot;Missing a temporary folder&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">UPLOAD_ERR_CANT_WRITE</span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$message </span><span class="keyword">= </span><span class="string">&quot;Failed to write file to disk&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">UPLOAD_ERR_EXTENSION</span><span class="keyword">:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$message </span><span class="keyword">= </span><span class="string">&quot;File upload stopped by extension&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; default:
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$message </span><span class="keyword">= </span><span class="string">&quot;Unknown upload error&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$message</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>}
<br>
<br></span><span class="comment">// Use
<br> </span><span class="keyword">if (</span><span class="default">$_FILES</span><span class="keyword">[</span><span class="string">&apos;file&apos;</span><span class="keyword">][</span><span class="string">&apos;error&apos;</span><span class="keyword">] === </span><span class="default">UPLOAD_ERR_OK</span><span class="keyword">) {
<br></span><span class="comment">//uploading successfully done
<br></span><span class="keyword">} else {
<br>throw new </span><span class="default">UploadException</span><span class="keyword">(</span><span class="default">$_FILES</span><span class="keyword">[</span><span class="string">&apos;file&apos;</span><span class="keyword">][</span><span class="string">&apos;error&apos;</span><span class="keyword">]);
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
This is probably useful to someone.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">array(
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">0</span><span class="keyword">=&gt;</span><span class="string">&quot;There is no error, the file uploaded with success&quot;</span><span class="keyword">, 
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">1</span><span class="keyword">=&gt;</span><span class="string">&quot;The uploaded file exceeds the upload_max_filesize directive in php.ini&quot;</span><span class="keyword">, 
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">2</span><span class="keyword">=&gt;</span><span class="string">&quot;The uploaded file exceeds the MAX_FILE_SIZE directive that was specified in the HTML form&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">3</span><span class="keyword">=&gt;</span><span class="string">&quot;The uploaded file was only partially uploaded&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">4</span><span class="keyword">=&gt;</span><span class="string">&quot;No file was uploaded&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">6</span><span class="keyword">=&gt;</span><span class="string">&quot;Missing a temporary folder&quot; 
<br></span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
if post is greater than post_max_size set in php.ini
<br>
<br>$_FILES and $_POST will return empty</span>
</div>
  

#


<div class="phpcode"><span class="html">
One thing that is annoying is that the way these constant values are handled requires processing no error with the equality, which wastes a little bit of space.&#xA0; Even though &quot;no error&quot; is 0, which typically evaluates to &quot;false&quot; in an if statement, it will always evaluate to true in this context.
<br>
<br>So, instead of this:
<br>-----
<br><span class="default">&lt;?php
<br></span><span class="keyword">if(</span><span class="default">$_FILES</span><span class="keyword">[</span><span class="string">&apos;userfile&apos;</span><span class="keyword">][</span><span class="string">&apos;error&apos;</span><span class="keyword">]) {
<br>&#xA0; </span><span class="comment">// handle the error
<br></span><span class="keyword">} else {
<br>&#xA0; </span><span class="comment">// process
<br></span><span class="keyword">}
<br></span><span class="default">?&gt;
<br></span>-----
<br>You have to do this:
<br>-----
<br><span class="default">&lt;?php
<br></span><span class="keyword">if(</span><span class="default">$_FILES</span><span class="keyword">[</span><span class="string">&apos;userfile&apos;</span><span class="keyword">][</span><span class="string">&apos;error&apos;</span><span class="keyword">]==</span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; </span><span class="comment">// process
<br></span><span class="keyword">} else {
<br>&#xA0; </span><span class="comment">// handle the error
<br></span><span class="keyword">}
<br></span><span class="default">?&gt;
<br></span>-----
<br>Also, ctype_digit fails, but is_int works.&#xA0; If you&apos;re wondering... no, it doesn&apos;t make any sense.
<br>
<br>To Schoschie:
<br>
<br>You ask the question:&#xA0; Why make stuff complicated when you can make it easy?&#xA0; I ask the same question since the version of the code you / Anonymous / Thalent (per danbrown) have posted is unnecessary overhead and would result in a function call, as well as a potentially lengthy switch statement.&#xA0; In a loop, that would be deadly... try this instead:
<br>
<br>-----
<br><span class="default">&lt;?php
<br>$error_types </span><span class="keyword">= array(
<br></span><span class="default">1</span><span class="keyword">=&gt;</span><span class="string">&apos;The uploaded file exceeds the upload_max_filesize directive in php.ini.&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;The uploaded file exceeds the MAX_FILE_SIZE directive that was specified in the HTML form.&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;The uploaded file was only partially uploaded.&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;No file was uploaded.&apos;</span><span class="keyword">,
<br></span><span class="default">6</span><span class="keyword">=&gt;</span><span class="string">&apos;Missing a temporary folder.&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;Failed to write file to disk.&apos;</span><span class="keyword">,
<br></span><span class="string">&apos;A PHP extension stopped the file upload.&apos;
<br></span><span class="keyword">);
<br>
<br></span><span class="comment">// Outside a loop...
<br></span><span class="keyword">if(</span><span class="default">$_FILES</span><span class="keyword">[</span><span class="string">&apos;userfile&apos;</span><span class="keyword">][</span><span class="string">&apos;error&apos;</span><span class="keyword">]==</span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; </span><span class="comment">// process
<br></span><span class="keyword">} else {
<br>&#xA0; </span><span class="default">$error_message </span><span class="keyword">= </span><span class="default">$error_types</span><span class="keyword">[</span><span class="default">$_FILES</span><span class="keyword">[</span><span class="string">&apos;userfile&apos;</span><span class="keyword">][</span><span class="string">&apos;error&apos;</span><span class="keyword">]];
<br>&#xA0; </span><span class="comment">// do whatever with the error message
<br></span><span class="keyword">}
<br>
<br></span><span class="comment">// In a loop...
<br></span><span class="keyword">for(</span><span class="default">$x</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">,</span><span class="default">$y</span><span class="keyword">=</span><span class="default">count</span><span class="keyword">(</span><span class="default">$_FILES</span><span class="keyword">[</span><span class="string">&apos;userfile&apos;</span><span class="keyword">][</span><span class="string">&apos;error&apos;</span><span class="keyword">]);</span><span class="default">$x</span><span class="keyword">&lt;</span><span class="default">$y</span><span class="keyword">;++</span><span class="default">$x</span><span class="keyword">) {
<br>&#xA0; if(</span><span class="default">$_FILES</span><span class="keyword">[</span><span class="string">&apos;userfile&apos;</span><span class="keyword">][</span><span class="string">&apos;error&apos;</span><span class="keyword">][</span><span class="default">$x</span><span class="keyword">]==</span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="comment">// process
<br>&#xA0; </span><span class="keyword">} else {
<br>&#xA0; &#xA0; </span><span class="default">$error_message </span><span class="keyword">= </span><span class="default">$error_types</span><span class="keyword">[</span><span class="default">$_FILES</span><span class="keyword">[</span><span class="string">&apos;userfile&apos;</span><span class="keyword">][</span><span class="string">&apos;error&apos;</span><span class="keyword">][</span><span class="default">$x</span><span class="keyword">]];
<br>&#xA0; &#xA0; </span><span class="comment">// Do whatever with the error message
<br>&#xA0; </span><span class="keyword">}
<br>}
<br>
<br></span><span class="comment">// When you&apos;re done... if you aren&apos;t doing all of this in a function that&apos;s about to end / complete all the processing and want to reclaim the memory
<br></span><span class="keyword">unset(</span><span class="default">$error_types</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/features.file-upload.errors.php)

**[To root](/README.md)**