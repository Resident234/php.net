# fsockopen




<div class="phpcode"><span class="html">
just a quick note for users attempting https and thinking they must resort to curl or alternate methods -
<br>you can use fsockopen, just read the docs closely.&#xA0; basically they are saying to use &apos;ssl://&apos; for a HTTPS (SSL) web request.
<br>
<br>so this would work for authorize.net, and others; even for that paypal IPN - however I think it would be best to leave the site and deal with paypal&apos;s form:
<br>
<br><span class="default">&lt;?php
<br>$host </span><span class="keyword">= </span><span class="string">&quot;something.example.com&quot;</span><span class="keyword">;
<br></span><span class="default">$port </span><span class="keyword">= </span><span class="default">443</span><span class="keyword">;
<br></span><span class="default">$path </span><span class="keyword">= </span><span class="string">&quot;/the/url/path/file.php&quot;</span><span class="keyword">; </span><span class="comment">//or .dll, etc. for authnet, etc.
<br>
<br>//you will need to setup an array of fields to post with
<br>//then create the post string
<br></span><span class="default">$formdata </span><span class="keyword">= array ( </span><span class="string">&quot;x_field&quot; </span><span class="keyword">=&gt; </span><span class="string">&quot;somevalue&quot;</span><span class="keyword">);
<br></span><span class="comment">//build the post string
<br>&#xA0; </span><span class="keyword">foreach(</span><span class="default">$formdata </span><span class="keyword">AS </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$val</span><span class="keyword">){
<br>&#xA0; &#xA0; </span><span class="default">$poststring </span><span class="keyword">.= </span><span class="default">urlencode</span><span class="keyword">(</span><span class="default">$key</span><span class="keyword">) . </span><span class="string">&quot;=&quot; </span><span class="keyword">. </span><span class="default">urlencode</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">) . </span><span class="string">&quot;&amp;&quot;</span><span class="keyword">;
<br>&#xA0; }
<br></span><span class="comment">// strip off trailing ampersand
<br></span><span class="default">$poststring </span><span class="keyword">= </span><span class="default">substr</span><span class="keyword">(</span><span class="default">$poststring</span><span class="keyword">, </span><span class="default">0</span><span class="keyword">, -</span><span class="default">1</span><span class="keyword">);
<br>
<br></span><span class="default">$fp </span><span class="keyword">= </span><span class="default">fsockopen</span><span class="keyword">(</span><span class="string">&quot;ssl://&quot;</span><span class="keyword">.</span><span class="default">$host</span><span class="keyword">, </span><span class="default">$port</span><span class="keyword">, </span><span class="default">$errno</span><span class="keyword">, </span><span class="default">$errstr</span><span class="keyword">, </span><span class="default">$timeout </span><span class="keyword">= </span><span class="default">30</span><span class="keyword">);
<br>
<br>if(!</span><span class="default">$fp</span><span class="keyword">){
<br> </span><span class="comment">//error tell us
<br> </span><span class="keyword">echo </span><span class="string">&quot;</span><span class="default">$errstr</span><span class="string"> (</span><span class="default">$errno</span><span class="string">)\n&quot;</span><span class="keyword">;
<br>&#xA0;&#xA0; 
<br>}else{
<br>
<br>&#xA0; </span><span class="comment">//send the server request
<br>&#xA0; </span><span class="default">fputs</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="string">&quot;POST </span><span class="default">$path</span><span class="string"> HTTP/1.1\r\n&quot;</span><span class="keyword">);
<br>&#xA0; </span><span class="default">fputs</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="string">&quot;Host: </span><span class="default">$host</span><span class="string">\r\n&quot;</span><span class="keyword">);
<br>&#xA0; </span><span class="default">fputs</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="string">&quot;Content-type: application/x-www-form-urlencoded\r\n&quot;</span><span class="keyword">);
<br>&#xA0; </span><span class="default">fputs</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="string">&quot;Content-length: &quot;</span><span class="keyword">.</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$poststring</span><span class="keyword">).</span><span class="string">&quot;\r\n&quot;</span><span class="keyword">);
<br>&#xA0; </span><span class="default">fputs</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="string">&quot;Connection: close\r\n\r\n&quot;</span><span class="keyword">);
<br>&#xA0; </span><span class="default">fputs</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">$poststring </span><span class="keyword">. </span><span class="string">&quot;\r\n\r\n&quot;</span><span class="keyword">);
<br>
<br>&#xA0; </span><span class="comment">//loop through the response from the server
<br>&#xA0; </span><span class="keyword">while(!</span><span class="default">feof</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">)) {
<br>&#xA0; &#xA0; echo </span><span class="default">fgets</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">4096</span><span class="keyword">);
<br>&#xA0; }
<br>&#xA0; </span><span class="comment">//close fp - we are done with it
<br>&#xA0; </span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php
<br></span><span class="comment">// This script is an example of posting multiple files using 
<br>// fsockopen.
<br>// The tricky part is making sure the HTTP headers and file boundaries are acceptable to the target webserver.
<br>// This script is for example purposes only and could/should be improved upon.
<br>
<br></span><span class="default">$host</span><span class="keyword">=</span><span class="string">&apos;targethost&apos;</span><span class="keyword">;
<br></span><span class="default">$port</span><span class="keyword">=</span><span class="default">80</span><span class="keyword">;
<br></span><span class="default">$path</span><span class="keyword">=</span><span class="string">&apos;/test/socket/file_upload/receive_files.php&apos;</span><span class="keyword">;
<br>
<br></span><span class="comment">// the file you want to upload 
<br></span><span class="default">$file_array</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] = </span><span class="string">&quot;dingoboy.gif&quot;</span><span class="keyword">; </span><span class="comment">// the file 
<br></span><span class="default">$file_array</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">] = </span><span class="string">&quot;dingoboy2.gif&quot;</span><span class="keyword">; </span><span class="comment">// the file 
<br></span><span class="default">$file_array</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">] = </span><span class="string">&quot;dingoboy3.gif&quot;</span><span class="keyword">; </span><span class="comment">// the file 
<br></span><span class="default">$content_type </span><span class="keyword">= </span><span class="string">&quot;image/gif&quot;</span><span class="keyword">; </span><span class="comment">// the file mime type
<br>//$content_type = &quot;text/plain&quot;;
<br>//echo &quot;file_array[0]:$file_array[0]&lt;br&gt;&lt;br&gt;&quot;;
<br>
<br></span><span class="default">srand</span><span class="keyword">((double)</span><span class="default">microtime</span><span class="keyword">()*</span><span class="default">1000000</span><span class="keyword">);
<br></span><span class="default">$boundary </span><span class="keyword">= </span><span class="string">&quot;---------------------------&quot;</span><span class="keyword">.</span><span class="default">substr</span><span class="keyword">(</span><span class="default">md5</span><span class="keyword">(</span><span class="default">rand</span><span class="keyword">(</span><span class="default">0</span><span class="keyword">,</span><span class="default">32000</span><span class="keyword">)),</span><span class="default">0</span><span class="keyword">,</span><span class="default">10</span><span class="keyword">);
<br>
<br></span><span class="default">$data </span><span class="keyword">= </span><span class="string">&quot;--</span><span class="default">$boundary</span><span class="string">&quot;</span><span class="keyword">;
<br>
<br>for(</span><span class="default">$i</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">;</span><span class="default">$i</span><span class="keyword">&lt;</span><span class="default">count</span><span class="keyword">(</span><span class="default">$file_array</span><span class="keyword">);</span><span class="default">$i</span><span class="keyword">++){
<br>&#xA0;&#xA0; </span><span class="default">$content_file </span><span class="keyword">= </span><span class="default">join</span><span class="keyword">(</span><span class="string">&quot;&quot;</span><span class="keyword">, </span><span class="default">file</span><span class="keyword">(</span><span class="default">$file_array</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]));
<br>
<br>&#xA0;&#xA0; </span><span class="default">$data</span><span class="keyword">.=</span><span class="string">&quot;
<br>Content-Disposition: form-data; name=\&quot;file&quot;</span><span class="keyword">.(</span><span class="default">$i</span><span class="keyword">+</span><span class="default">1</span><span class="keyword">).</span><span class="string">&quot;\&quot;; filename=\&quot;</span><span class="default">$file_array</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">]</span><span class="string">\&quot;
<br>Content-Type: </span><span class="default">$content_type</span><span class="string"> 
<br>
<br></span><span class="default">$content_file</span><span class="string">
<br>--</span><span class="default">$boundary</span><span class="string">&quot;</span><span class="keyword">;
<br>
<br>}
<br>
<br></span><span class="default">$data</span><span class="keyword">.=</span><span class="string">&quot;--\r\n\r\n&quot;</span><span class="keyword">;
<br>
<br></span><span class="default">$msg </span><span class="keyword">=
<br></span><span class="string">&quot;POST </span><span class="default">$path</span><span class="string"> HTTP/1.0
<br>Content-Type: multipart/form-data; boundary=</span><span class="default">$boundary</span><span class="string">
<br>Content-Length: &quot;</span><span class="keyword">.</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">).</span><span class="string">&quot;\r\n\r\n&quot;</span><span class="keyword">;
<br>
<br></span><span class="default">$result</span><span class="keyword">=</span><span class="string">&quot;&quot;</span><span class="keyword">;
<br>
<br></span><span class="comment">// open the connection
<br></span><span class="default">$f </span><span class="keyword">= </span><span class="default">fsockopen</span><span class="keyword">(</span><span class="default">$host</span><span class="keyword">, </span><span class="default">$port</span><span class="keyword">);
<br>
<br></span><span class="default">fputs</span><span class="keyword">(</span><span class="default">$f</span><span class="keyword">,</span><span class="default">$msg</span><span class="keyword">.</span><span class="default">$data</span><span class="keyword">);
<br>
<br></span><span class="comment">// get the response
<br></span><span class="keyword">while (!</span><span class="default">feof</span><span class="keyword">(</span><span class="default">$f</span><span class="keyword">)) </span><span class="default">$result </span><span class="keyword">.= </span><span class="default">fread</span><span class="keyword">(</span><span class="default">$f</span><span class="keyword">,</span><span class="default">32000</span><span class="keyword">);
<br>
<br></span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$f</span><span class="keyword">);
<br>
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
This script checks specific ports so you need to have the correct port open on the server for this to work.
<br>
<br>E.g if i have a windows domain controller and it is servering LDAP then the following would be used to check it is online:
<br><span class="default">&lt;?php
<br>chkServer</span><span class="keyword">(</span><span class="string">&quot;MyDC&quot;</span><span class="keyword">, </span><span class="string">&quot;389&quot;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>for a webserver:
<br><span class="default">&lt;?php
<br>chkServer</span><span class="keyword">(</span><span class="string">&quot;MyWebSvr&quot;</span><span class="keyword">, </span><span class="string">&quot;80&quot;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>etc etc
<br>--------------------------------------------------------
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">// check if a server is up by connecting to a port
<br></span><span class="keyword">function </span><span class="default">chkServer</span><span class="keyword">(</span><span class="default">$host</span><span class="keyword">, </span><span class="default">$port</span><span class="keyword">)
<br>{&#xA0;&#xA0; 
<br>&#xA0; &#xA0; </span><span class="default">$hostip </span><span class="keyword">= @</span><span class="default">gethostbyname</span><span class="keyword">(</span><span class="default">$host</span><span class="keyword">); </span><span class="comment">// resloves IP from Hostname returns hostname on failure
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">$hostip </span><span class="keyword">== </span><span class="default">$host</span><span class="keyword">) </span><span class="comment">// if the IP is not resloved
<br>&#xA0; &#xA0; </span><span class="keyword">{
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;Server is down or does not exist&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; else
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">$x </span><span class="keyword">= @</span><span class="default">fsockopen</span><span class="keyword">(</span><span class="default">$hostip</span><span class="keyword">, </span><span class="default">$port</span><span class="keyword">, </span><span class="default">$errno</span><span class="keyword">, </span><span class="default">$errstr</span><span class="keyword">, </span><span class="default">5</span><span class="keyword">)) </span><span class="comment">// attempt to connect
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">{
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;Server is down&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; else 
<br>&#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;Server is up&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$x</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; @</span><span class="default">fclose</span><span class="keyword">(</span><span class="default">$x</span><span class="keyword">); </span><span class="comment">//close connection
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}
<br>&#xA0; &#xA0; &#xA0; &#xA0; }&#xA0; 
<br>&#xA0; &#xA0; }
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.fsockopen.php)

**[To root](/README.md)**