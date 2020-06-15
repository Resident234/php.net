# Session Upload Progress




<div class="phpcode"><span class="html">
Note, this feature doesn&apos;t work, when your webserver is runnig PHP via FastCGI. There will be no progress informations in the session array.<br>Unfortunately PHP gets the data only after the upload is completed and can&apos;t show any progress.<br><br>I hope this informations helps.</span>
</div>
  

#


<div class="phpcode"><span class="html">
ATTENTION:<br><br>Put the upload progress session name input field BEFORE your file field in the form :<br><br>&#xA0; &lt;form action=&quot;upload.php&quot; method=&quot;POST&quot; enctype=&quot;multipart/form-data&quot;&gt;<br>&#xA0; &lt;input type=&quot;hidden&quot; name=&quot;<span class="default">&lt;?php </span><span class="keyword">echo </span><span class="default">ini_get</span><span class="keyword">(</span><span class="string">&quot;session.upload_progress.name&quot;</span><span class="keyword">); </span><span class="default">?&gt;</span>&quot; value=&quot;123&quot; /&gt;<br>&#xA0; &lt;input type=&quot;file&quot; name=&quot;file1&quot; /&gt;<br>&#xA0; &lt;input type=&quot;file&quot; name=&quot;file2&quot; /&gt;<br>&#xA0; &lt;input type=&quot;submit&quot; /&gt;<br>&#xA0; &lt;/form&gt;<br><br>If you make it after your file field, you&apos;ll waste a lot of time figuring why (just like me ...)<br><br>The following form will make you crazy and waste really a lot of time:<br><br>&lt;form action=&quot;upload.php&quot; method=&quot;POST&quot; enctype=&quot;multipart/form-data&quot;&gt;<br> &lt;input type=&quot;file&quot; name=&quot;file1&quot; /&gt;<br> &lt;input type=&quot;file&quot; name=&quot;file2&quot; /&gt;<br> &lt;input type=&quot;hidden&quot; name=&quot;<span class="default">&lt;?php </span><span class="keyword">echo </span><span class="default">ini_get</span><span class="keyword">(</span><span class="string">&quot;session.upload_progress.name&quot;</span><span class="keyword">); </span><span class="default">?&gt;</span>&quot; value=&quot;123&quot; /&gt;<br> &lt;input type=&quot;submit&quot; /&gt;<br>&lt;/form&gt;<br><br>DON&apos;T do this!</span>
</div>
  

#


<div class="phpcode"><span class="html">
While the example in the documentation is accurate, the description is a bit off. To clarify:<br><br>PHP will populate an array in the $_SESSION, where the index is a concatenated value of the session.upload_progress.prefix and the VALUE of the POSTed session.upload_progress.name variable.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you&apos;re seeing<br>&quot;PHP Warning:&#xA0; Unknown: The session id is too long or contains illegal characters, valid characters are a-z, A-Z, 0-9 and &apos;-,&apos; in Unknown on line 0&quot;,<br>then a misplaced input could be the cause. It&apos;s worth mentioning again that the hidden element MUST be before the file elements.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/session.upload-progress.php)

**[To root](/README.md)**