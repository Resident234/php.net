# PUT method support




<div class="phpcode"><span class="html">
Hello PHP World After many Hours of worryness :=)
<br>
<br>I have found the Solution for Resume or Pause Uploads
<br>In this Code Snippet it is the Server Side not Client on any Desktop Programm you must use byte ranges to calculate the uploaded bytes and missing of total bytes.
<br>
<br>Here the PHP Code
<br>
<br><span class="default">&lt;?php
<br>$CHUNK </span><span class="keyword">= </span><span class="default">8192</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; try {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!(</span><span class="default">$putData </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="string">&quot;php://input&quot;</span><span class="keyword">, </span><span class="string">&quot;r&quot;</span><span class="keyword">)))
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&quot;Can&apos;t get PUT data.&quot;</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// now the params can be used like any other variable
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // see below after input has finished
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$tot_write </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$tmpFileName </span><span class="keyword">= </span><span class="string">&quot;/var/dev/tmp/PUT_FILE&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Create a temp file
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if (!</span><span class="default">is_file</span><span class="keyword">(</span><span class="default">$tmpFileName</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$tmpFileName</span><span class="keyword">, </span><span class="string">&quot;x&quot;</span><span class="keyword">)); </span><span class="comment">//create the file and close it
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Open the file for writing
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if (!(</span><span class="default">$fp </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$tmpFileName</span><span class="keyword">, </span><span class="string">&quot;w&quot;</span><span class="keyword">)))
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&quot;Can&apos;t write to tmp file&quot;</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Read the data a chunk at a time and write to the file
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">while (</span><span class="default">$data </span><span class="keyword">= </span><span class="default">fread</span><span class="keyword">(</span><span class="default">$putData</span><span class="keyword">, </span><span class="default">$CHUNK</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$chunk_read </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ((</span><span class="default">$block_write </span><span class="keyword">= </span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">$data</span><span class="keyword">)) != </span><span class="default">$chunk_read</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&quot;Can&apos;t write more to tmp file&quot;</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$tot_write </span><span class="keyword">+= </span><span class="default">$block_write</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">))
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&quot;Can&apos;t close tmp file&quot;</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; unset(</span><span class="default">$putData</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Open the file for writing
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if (!(</span><span class="default">$fp </span><span class="keyword">= </span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$tmpFileName</span><span class="keyword">, </span><span class="string">&quot;a&quot;</span><span class="keyword">)))
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&quot;Can&apos;t write to tmp file&quot;</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Read the data a chunk at a time and write to the file
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">while (</span><span class="default">$data </span><span class="keyword">= </span><span class="default">fread</span><span class="keyword">(</span><span class="default">$putData</span><span class="keyword">, </span><span class="default">$CHUNK</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$chunk_read </span><span class="keyword">= </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if ((</span><span class="default">$block_write </span><span class="keyword">= </span><span class="default">fwrite</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">$data</span><span class="keyword">)) != </span><span class="default">$chunk_read</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&quot;Can&apos;t write more to tmp file&quot;</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$tot_write </span><span class="keyword">+= </span><span class="default">$block_write</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">))
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&quot;Can&apos;t close tmp file&quot;</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; unset(</span><span class="default">$putData</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Check file length and MD5
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">$tot_write </span><span class="keyword">!= </span><span class="default">$file_size</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&quot;Wrong file size&quot;</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$md5_arr </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos; &apos;</span><span class="keyword">, </span><span class="default">exec</span><span class="keyword">(</span><span class="string">&quot;md5sum </span><span class="default">$tmpFileName</span><span class="string">&quot;</span><span class="keyword">));
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$md5 </span><span class="keyword">= </span><span class="default">$md5sum_arr</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$md5 </span><span class="keyword">!= </span><span class="default">$md5sum</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&quot;Wrong md5&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; } catch (</span><span class="default">Exception $e</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="default">$e</span><span class="keyword">-&gt;</span><span class="default">getMessage</span><span class="keyword">(), </span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/features.file-upload.put-method.php)

**[To root](/README.md)**