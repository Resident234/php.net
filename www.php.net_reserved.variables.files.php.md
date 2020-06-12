# $_FILES




<div class="phpcode"><span class="html">
see <a href="http://php.net/manual/en/features.file-upload.post-method.php" rel="nofollow" target="_blank">http://php.net/manual/en/features.file-upload.post-method.php</a> for documentation of the $_FILES array, which is what I came to this page for in the first place.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you are looking for the $_FILES[&apos;error&apos;] code explanations, be sure to read:<br><br>Handling File Uploads - Error Messages Explained<br><a href="http://www.php.net/manual/en/features.file-upload.errors.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/features.file-upload.errors.php</a></span>
</div>
  

#


<div class="phpcode"><span class="html">
A note of security: Don&apos;t ever trust $_FILES[&quot;image&quot;][&quot;type&quot;]. It takes whatever is sent from the browser, so don&apos;t trust this for the image type.&#xA0; I recommend using finfo_open (<a href="http://www.php.net/manual/en/function.finfo-open.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/function.finfo-open.php</a>) to verify the MIME type of a file. It will parse the MAGIC in the file and return it&apos;s type...this can be trusted (you can also use the &quot;file&quot; program on Unix, but I would refrain from ever making a System call with your PHP code...that&apos;s just asking for problems).</span>
</div>
  

#


<div class="phpcode"><span class="html">
The format of this array is (assuming your form has two input type=file fields named &quot;file1&quot;, &quot;file2&quot;, etc):<br><br>Array<br>(<br>&#xA0; &#xA0; [file1] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; MyFile.txt (comes from the browser, so treat as tainted)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [type] =&gt; text/plain&#xA0; (not sure where it gets this from - assume the browser, so treat as tainted)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [tmp_name] =&gt; /tmp/php/php1h4j1o (could be anywhere on your system, depending on your config settings, but the user has no control, so this isn&apos;t tainted)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [error] =&gt; UPLOAD_ERR_OK&#xA0; (= 0)<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [size] =&gt; 123&#xA0;&#xA0; (the size in bytes)<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; [file2] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; MyFile.jpg<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [type] =&gt; image/jpeg<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [tmp_name] =&gt; /tmp/php/php6hst32<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [error] =&gt; UPLOAD_ERR_OK<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [size] =&gt; 98174<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br>)<br><br>Last I checked (a while ago now admittedly), if you use array parameters in your forms (that is, form names ending in square brackets, like several file fields called &quot;download[file1]&quot;, &quot;download[file2]&quot; etc), then the array format becomes... interesting.<br><br>Array<br>(<br>&#xA0; &#xA0; [download] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [name] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file1] =&gt; MyFile.txt<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file2] =&gt; MyFile.jpg<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [type] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file1] =&gt; text/plain<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file2] =&gt; image/jpeg<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [tmp_name] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file1] =&gt; /tmp/php/php1h4j1o<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file2] =&gt; /tmp/php/php6hst32<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [error] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file1] =&gt; UPLOAD_ERR_OK<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file2] =&gt; UPLOAD_ERR_OK<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br><br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [size] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file1] =&gt; 123<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [file2] =&gt; 98174<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br>)<br><br>So you&apos;d need to access the error param of file1 as, eg $_Files[&apos;download&apos;][&apos;error&apos;][&apos;file1&apos;]</span>
</div>
  

#


<div class="phpcode"><span class="html">
A nice trick to reorder the $_FILES array when you use a input name as array is:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">diverse_array</span><span class="keyword">(</span><span class="default">$vector</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= array();
<br>&#xA0; &#xA0; foreach(</span><span class="default">$vector </span><span class="keyword">as </span><span class="default">$key1 </span><span class="keyword">=&gt; </span><span class="default">$value1</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach(</span><span class="default">$value1 </span><span class="keyword">as </span><span class="default">$key2 </span><span class="keyword">=&gt; </span><span class="default">$value2</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$result</span><span class="keyword">[</span><span class="default">$key2</span><span class="keyword">][</span><span class="default">$key1</span><span class="keyword">] = </span><span class="default">$value2</span><span class="keyword">;
<br>&#xA0; &#xA0; return </span><span class="default">$result</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>will transform this:
<br>
<br>array(1) {
<br>&#xA0; &#xA0; [&quot;upload&quot;]=&gt;array(2) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; [&quot;name&quot;]=&gt;array(2) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0]=&gt;string(9)&quot;file0.txt&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1]=&gt;string(9)&quot;file1.txt&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; [&quot;type&quot;]=&gt;array(2) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0]=&gt;string(10)&quot;text/plain&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1]=&gt;string(10)&quot;text/html&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>}
<br>
<br>into:
<br>
<br>array(1) {
<br>&#xA0; &#xA0; [&quot;upload&quot;]=&gt;array(2) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; [0]=&gt;array(2) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [&quot;name&quot;]=&gt;string(9)&quot;file0.txt&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [&quot;type&quot;]=&gt;string(10)&quot;text/plain&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; },
<br>&#xA0; &#xA0; &#xA0; &#xA0; [1]=&gt;array(2) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [&quot;name&quot;]=&gt;string(9)&quot;file1.txt&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [&quot;type&quot;]=&gt;string(10)&quot;text/html&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>}
<br>
<br>just do:
<br>
<br><span class="default">&lt;?php $upload </span><span class="keyword">= </span><span class="default">diverse_array</span><span class="keyword">(</span><span class="default">$_FILES</span><span class="keyword">[</span><span class="string">&quot;upload&quot;</span><span class="keyword">]); </span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If $_FILES is empty, even when uploading, try adding enctype=&quot;multipart/form-data&quot; to the form tag and make sure you have file uploads turned on.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.files.php)

**[To root](/)**