# xml_parse




<div class="phpcode"><span class="html">
Instead of passing a URL, we can pass the XML content to this class (either you
<br> want to use CURL, Socks or fopen to retrieve it first) and instead of using 
<br> array, I&apos;m using separator &apos;|&apos; to identify which data to get (in order to make 
<br> it short to retrieve a complex XML data). Here is my class with built-in fopen 
<br> which you can pass URL or you can pass the content instead :
<br>
<br>p/s : thanks to this great help page.
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="keyword">class </span><span class="default">xx_xml </span><span class="keyword">{
<br>
<br>&#xA0; &#xA0; </span><span class="comment">// XML parser variables
<br>&#xA0; &#xA0; </span><span class="keyword">var </span><span class="default">$parser</span><span class="keyword">;
<br>&#xA0; &#xA0; var </span><span class="default">$name</span><span class="keyword">;
<br>&#xA0; &#xA0; var </span><span class="default">$attr</span><span class="keyword">;
<br>&#xA0; &#xA0; var </span><span class="default">$data&#xA0; </span><span class="keyword">= array();
<br>&#xA0; &#xA0; var </span><span class="default">$stack </span><span class="keyword">= array();
<br>&#xA0; &#xA0; var </span><span class="default">$keys</span><span class="keyword">;
<br>&#xA0; &#xA0; var </span><span class="default">$path</span><span class="keyword">;
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="comment">// either you pass url atau contents. 
<br>&#xA0; &#xA0; // Use &apos;url&apos; or &apos;contents&apos; for the parameter
<br>&#xA0; &#xA0; </span><span class="keyword">var </span><span class="default">$type</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; </span><span class="comment">// function with the default parameter value
<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">xx_xml</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">=</span><span class="string">&apos;<a href="http://www.example.com" rel="nofollow" target="_blank">http://www.example.com</a>&apos;</span><span class="keyword">, </span><span class="default">$type</span><span class="keyword">=</span><span class="string">&apos;url&apos;</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">type </span><span class="keyword">= </span><span class="default">$type</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">url&#xA0; </span><span class="keyword">= </span><span class="default">$url</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parse</span><span class="keyword">();
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="comment">// parse XML data
<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">parse</span><span class="keyword">()
<br>&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$data </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser </span><span class="keyword">= </span><span class="default">xml_parser_create</span><span class="keyword">();
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">xml_set_object</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">xml_set_element_handler</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser</span><span class="keyword">, </span><span class="string">&apos;startXML&apos;</span><span class="keyword">, </span><span class="string">&apos;endXML&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">xml_set_character_data_handler</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser</span><span class="keyword">, </span><span class="string">&apos;charXML&apos;</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">xml_parser_set_option</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser</span><span class="keyword">, </span><span class="default">XML_OPTION_CASE_FOLDING</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">type </span><span class="keyword">== </span><span class="string">&apos;url&apos;</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// if use type = &apos;url&apos; now we open the XML with fopen
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">if (!(</span><span class="default">$fp </span><span class="keyword">= @</span><span class="default">fopen</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">url</span><span class="keyword">, </span><span class="string">&apos;rb&apos;</span><span class="keyword">))) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">error</span><span class="keyword">(</span><span class="string">&quot;Cannot open </span><span class="keyword">{</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">url</span><span class="keyword">}</span><span class="string">&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; while ((</span><span class="default">$data </span><span class="keyword">= </span><span class="default">fread</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">, </span><span class="default">8192</span><span class="keyword">))) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">xml_parse</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser</span><span class="keyword">, </span><span class="default">$data</span><span class="keyword">, </span><span class="default">feof</span><span class="keyword">(</span><span class="default">$fp</span><span class="keyword">))) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">error</span><span class="keyword">(</span><span class="default">sprintf</span><span class="keyword">(</span><span class="string">&apos;XML error at line %d column %d&apos;</span><span class="keyword">, 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">xml_get_current_line_number</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser</span><span class="keyword">), 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">xml_get_current_column_number</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser</span><span class="keyword">)));
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; } else if (</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">type </span><span class="keyword">== </span><span class="string">&apos;contents&apos;</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">// Now we can pass the contents, maybe if you want 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // to use CURL, SOCK or other method.
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$lines </span><span class="keyword">= </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&quot;\n&quot;</span><span class="keyword">,</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">url</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$lines </span><span class="keyword">as </span><span class="default">$val</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">trim</span><span class="keyword">(</span><span class="default">$val</span><span class="keyword">) == </span><span class="string">&apos;&apos;</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; continue;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$data </span><span class="keyword">= </span><span class="default">$val </span><span class="keyword">. </span><span class="string">&quot;\n&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">xml_parse</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser</span><span class="keyword">, </span><span class="default">$data</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">error</span><span class="keyword">(</span><span class="default">sprintf</span><span class="keyword">(</span><span class="string">&apos;XML error at line %d column %d&apos;</span><span class="keyword">, 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">xml_get_current_line_number</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser</span><span class="keyword">), 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">xml_get_current_column_number</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser</span><span class="keyword">)));
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; function </span><span class="default">startXML</span><span class="keyword">(</span><span class="default">$parser</span><span class="keyword">, </span><span class="default">$name</span><span class="keyword">, </span><span class="default">$attr</span><span class="keyword">)&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">stack</span><span class="keyword">[</span><span class="default">$name</span><span class="keyword">] = array();
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$keys </span><span class="keyword">= </span><span class="string">&apos;&apos;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$total </span><span class="keyword">= </span><span class="default">count</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">stack</span><span class="keyword">)-</span><span class="default">1</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$i</span><span class="keyword">=</span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">stack </span><span class="keyword">as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$val</span><span class="keyword">)&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">count</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">stack</span><span class="keyword">) &gt; </span><span class="default">1</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">$total </span><span class="keyword">== </span><span class="default">$i</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$keys </span><span class="keyword">.= </span><span class="default">$key</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$keys </span><span class="keyword">.= </span><span class="default">$key </span><span class="keyword">. </span><span class="string">&apos;|&apos;</span><span class="keyword">; </span><span class="comment">// The saparator
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">}
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; else
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$keys </span><span class="keyword">.= </span><span class="default">$key</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$i</span><span class="keyword">++;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">array_key_exists</span><span class="keyword">(</span><span class="default">$keys</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">data</span><span class="keyword">))&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">data</span><span class="keyword">[</span><span class="default">$keys</span><span class="keyword">][] = </span><span class="default">$attr</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }&#xA0; &#xA0; else
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">data</span><span class="keyword">[</span><span class="default">$keys</span><span class="keyword">] = </span><span class="default">$attr</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">keys </span><span class="keyword">= </span><span class="default">$keys</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; function </span><span class="default">endXML</span><span class="keyword">(</span><span class="default">$parser</span><span class="keyword">, </span><span class="default">$name</span><span class="keyword">)&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">end</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">stack</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">key</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">stack</span><span class="keyword">) == </span><span class="default">$name</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">array_pop</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">stack</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; function </span><span class="default">charXML</span><span class="keyword">(</span><span class="default">$parser</span><span class="keyword">, </span><span class="default">$data</span><span class="keyword">)&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; if (</span><span class="default">trim</span><span class="keyword">(</span><span class="default">$data</span><span class="keyword">) != </span><span class="string">&apos;&apos;</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">data</span><span class="keyword">[</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">keys</span><span class="keyword">][</span><span class="string">&apos;data&apos;</span><span class="keyword">][] = </span><span class="default">trim</span><span class="keyword">(</span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&quot;\n&quot;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="default">$data</span><span class="keyword">));
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; function </span><span class="default">error</span><span class="keyword">(</span><span class="default">$msg</span><span class="keyword">)&#xA0; &#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&quot;&lt;div align=\&quot;center\&quot;&gt;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &lt;font color=\&quot;red\&quot;&gt;&lt;b&gt;Error: </span><span class="default">$msg</span><span class="string">&lt;/b&gt;&lt;/font&gt;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &lt;/div&gt;&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; exit();
<br>&#xA0; &#xA0; }
<br>}
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>And example of retrieving XML data:
<br>p/s: example use to retrieve weather
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">include_once </span><span class="string">&quot;xx_xml.class.php&quot;</span><span class="keyword">;
<br>
<br></span><span class="comment">// Im using simple curl (the original is in class) to get the contents
<br>
<br></span><span class="default">$pageurl </span><span class="keyword">= </span><span class="string">&quot;<a href="http://xml.weather.yahoo.com/forecastrss?p=MYXX0008&amp;u=c" rel="nofollow" target="_blank">http://xml.weather.yahoo.com/forecastrss?p=MYXX0008&amp;u=c</a>&quot;</span><span class="keyword">;
<br></span><span class="default">curl_setopt</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_RETURNTRANSFER</span><span class="keyword">, </span><span class="default">1</span><span class="keyword">);
<br></span><span class="default">curl_setopt </span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">, </span><span class="default">CURLOPT_URL</span><span class="keyword">, </span><span class="default">$pageurl </span><span class="keyword">);
<br></span><span class="default">$thecontents </span><span class="keyword">= </span><span class="default">curl_exec </span><span class="keyword">( </span><span class="default">$ch </span><span class="keyword">);
<br></span><span class="default">curl_close</span><span class="keyword">(</span><span class="default">$ch</span><span class="keyword">);
<br>
<br></span><span class="comment">// We want to pass only a ready XML content instead of URL
<br>// But if you want to use URL , skip the curl functions above and use this 
<br>// $xx4 = new xx_xml(&quot;url here&quot;,&apos;url&apos;);
<br>
<br></span><span class="default">$xx4 </span><span class="keyword">= new </span><span class="default">xx_xml</span><span class="keyword">(</span><span class="default">$thecontents</span><span class="keyword">,</span><span class="string">&apos;contents&apos;</span><span class="keyword">); 
<br></span><span class="comment">// As you can see, we use saparator &apos;|&apos; instead of long array
<br></span><span class="default">$Code </span><span class="keyword">= </span><span class="default">$xx4</span><span class="keyword">-&gt;</span><span class="default">data </span><span class="keyword">[</span><span class="string">&apos;rss|channel|item|yweather:condition&apos;</span><span class="keyword">][</span><span class="string">&apos;code&apos;</span><span class="keyword">] ;
<br></span><span class="default">$Celcius </span><span class="keyword">= </span><span class="default">$xx4</span><span class="keyword">-&gt;</span><span class="default">data </span><span class="keyword">[</span><span class="string">&apos;rss|channel|item|yweather:condition&apos;</span><span class="keyword">][</span><span class="string">&apos;temp&apos;</span><span class="keyword">] ;
<br></span><span class="default">$Text </span><span class="keyword">= </span><span class="default">$xx4</span><span class="keyword">-&gt;</span><span class="default">data </span><span class="keyword">[</span><span class="string">&apos;rss|channel|item|yweather:condition&apos;</span><span class="keyword">][</span><span class="string">&apos;text&apos;</span><span class="keyword">] ;
<br></span><span class="default">$Cityname </span><span class="keyword">= </span><span class="default">$xx4</span><span class="keyword">-&gt;</span><span class="default">data </span><span class="keyword">[</span><span class="string">&apos;rss|channel|yweather:location&apos;</span><span class="keyword">][</span><span class="string">&apos;city&apos;</span><span class="keyword">] ;
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>Hope this helps.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.xml-parse.php)

**[⬆ to root](/)**