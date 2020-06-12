# Using PHP from the command line




<div class="phpcode"><span class="html">
You can easily parse command line arguments into the $_GET variable by using the parse_str() function.<br><br><span class="default">&lt;?php<br><br>parse_str</span><span class="keyword">(</span><span class="default">implode</span><span class="keyword">(</span><span class="string">&apos;&amp;&apos;</span><span class="keyword">, </span><span class="default">array_slice</span><span class="keyword">(</span><span class="default">$argv</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">)), </span><span class="default">$_GET</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>It behaves exactly like you&apos;d expect with cgi-php.<br><br>$ php -f somefile.php a=1 b[]=2 b[]=3<br><br>This will set $_GET[&apos;a&apos;] to &apos;1&apos; and $_GET[&apos;b&apos;] to array(&apos;2&apos;, &apos;3&apos;).<br><br>Even better, instead of putting that line in every file, take advantage of PHP&apos;s auto_prepend_file directive.&#xA0; Put that line in its own file and set the auto_prepend_file directive in your cli-specific php.ini like so:<br><br>auto_prepend_file = &quot;/etc/php/cli-php5.3/local.prepend.php&quot;<br><br>It will be automatically prepended to any PHP file run from the command line.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Just a note for people trying to use interactive mode from the commandline.<br><br>The purpose of interactive mode is to parse code snippits without actually leaving php, and it works like this:<br><br>[root@localhost php-4.3.4]# php -a<br>Interactive mode enabled<br><br><span class="default">&lt;?php </span><span class="keyword">echo </span><span class="string">&quot;hi!&quot;</span><span class="keyword">; </span><span class="default">?&gt;<br></span>&lt;note, here we would press CTRL-D to parse everything we&apos;ve entered so far&gt;<br>hi!<br><span class="default">&lt;?php </span><span class="keyword">exit(); </span><span class="default">?&gt;<br></span>&lt;ctrl-d here again&gt;<br>[root@localhost php-4.3.4]#<br><br>I noticed this somehow got ommited from the docs, hope it helps someone!</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to be interactive with the user and accept user input, all you need to do is read from stdin.&#xA0; <br><br><span class="default">&lt;?php<br></span><span class="keyword">echo </span><span class="string">&quot;Are you sure you want to do this?&#xA0; Type &apos;yes&apos; to continue: &quot;</span><span class="keyword">;<br></span><span class="default">$handle </span><span class="keyword">= </span><span class="default">fopen </span><span class="keyword">(</span><span class="string">&quot;php://stdin&quot;</span><span class="keyword">,</span><span class="string">&quot;r&quot;</span><span class="keyword">);<br></span><span class="default">$line </span><span class="keyword">= </span><span class="default">fgets</span><span class="keyword">(</span><span class="default">$handle</span><span class="keyword">);<br>if(</span><span class="default">trim</span><span class="keyword">(</span><span class="default">$line</span><span class="keyword">) != </span><span class="string">&apos;yes&apos;</span><span class="keyword">){<br>&#xA0; &#xA0; echo </span><span class="string">&quot;ABORTING!\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; exit;<br>}<br>echo </span><span class="string">&quot;\n&quot;</span><span class="keyword">;<br>echo </span><span class="string">&quot;Thank you, continuing...\n&quot;</span><span class="keyword">;<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/features.commandline.php)

**[To root](/)**