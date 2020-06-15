# XML Parser Functions




<div class="phpcode"><span class="html">
<span class="default">&lt;?php
<br></span><span class="comment">/**
<br> *&#xA0; XML to Associative Array Class
<br> * 
<br> *&#xA0; Usage:
<br> *&#xA0; &#xA0;&#xA0; $domObj = new xmlToArrayParser($xml);
<br> *&#xA0; &#xA0;&#xA0; $domArr = $domObj-&gt;array;
<br> *&#xA0; &#xA0;&#xA0; 
<br> *&#xA0; &#xA0;&#xA0; if($domObj-&gt;parse_error) echo $domObj-&gt;get_xml_error();
<br> *&#xA0; &#xA0;&#xA0; else print_r($domArr);
<br> * 
<br> *&#xA0; &#xA0;&#xA0; On Success: 
<br> *&#xA0; &#xA0;&#xA0; eg. $domArr[&apos;top&apos;][&apos;element2&apos;][&apos;attrib&apos;][&apos;var2&apos;] =&gt; val2
<br> * 
<br> *&#xA0; &#xA0;&#xA0; On Error:
<br> *&#xA0; &#xA0;&#xA0; eg. Error Code [76] &quot;Mismatched tag&quot;, at char 58 on line 3
<br> */
<br>
<br>/**
<br> * Convert an xml file or string to an associative array (including the tag attributes): 
<br> * $domObj = new xmlToArrayParser($xml); 
<br> * $elemVal = $domObj-&gt;array[&apos;element&apos;]
<br> * Or:&#xA0; $domArr=$domObj-&gt;array;&#xA0; $elemVal = $domArr[&apos;element&apos;].
<br> * 
<br> * @version&#xA0; 2.0
<br> * @param Str $xml file/string.
<br> */
<br></span><span class="keyword">class </span><span class="default">xmlToArrayParser </span><span class="keyword">{
<br>&#xA0; </span><span class="comment">/** The array created by the parser can be assigned to any variable: $anyVarArr = $domObj-&gt;array.*/
<br>&#xA0; </span><span class="keyword">public&#xA0; </span><span class="default">$array </span><span class="keyword">= array();
<br>&#xA0; public&#xA0; </span><span class="default">$parse_error </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">;
<br>&#xA0; private </span><span class="default">$parser</span><span class="keyword">;
<br>&#xA0; private </span><span class="default">$pointer</span><span class="keyword">;
<br>&#xA0; 
<br>&#xA0; </span><span class="comment">/** Constructor: $domObj = new xmlToArrayParser($xml); */
<br>&#xA0; </span><span class="keyword">public function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$xml</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer </span><span class="keyword">=&amp; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">array</span><span class="keyword">;
<br>&#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser </span><span class="keyword">= </span><span class="default">xml_parser_create</span><span class="keyword">(</span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">xml_set_object</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser</span><span class="keyword">, </span><span class="default">$this</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">xml_parser_set_option</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser</span><span class="keyword">, </span><span class="default">XML_OPTION_CASE_FOLDING</span><span class="keyword">, </span><span class="default">false</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">xml_set_element_handler</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser</span><span class="keyword">, </span><span class="string">&quot;tag_open&quot;</span><span class="keyword">, </span><span class="string">&quot;tag_close&quot;</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">xml_set_character_data_handler</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser</span><span class="keyword">, </span><span class="string">&quot;cdata&quot;</span><span class="keyword">); 
<br>&#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parse_error </span><span class="keyword">= </span><span class="default">xml_parse</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser</span><span class="keyword">, </span><span class="default">ltrim</span><span class="keyword">(</span><span class="default">$xml</span><span class="keyword">))? </span><span class="default">false </span><span class="keyword">: </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; }
<br>&#xA0; 
<br>&#xA0; </span><span class="comment">/** Free the parser. */
<br>&#xA0; </span><span class="keyword">public function </span><span class="default">__destruct</span><span class="keyword">() { </span><span class="default">xml_parser_free</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser</span><span class="keyword">);}
<br>
<br>&#xA0; </span><span class="comment">/** Get the xml error if an an error in the xml file occured during parsing. */
<br>&#xA0; </span><span class="keyword">public function </span><span class="default">get_xml_error</span><span class="keyword">() {
<br>&#xA0; &#xA0; if(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parse_error</span><span class="keyword">) { 
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$errCode </span><span class="keyword">= </span><span class="default">xml_get_error_code </span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$thisError </span><span class="keyword">=&#xA0; </span><span class="string">&quot;Error Code [&quot;</span><span class="keyword">. </span><span class="default">$errCode </span><span class="keyword">.</span><span class="string">&quot;] \&quot;&lt;strong style=&apos;color:red;&apos;&gt;&quot; </span><span class="keyword">. </span><span class="default">xml_error_string</span><span class="keyword">(</span><span class="default">$errCode</span><span class="keyword">).</span><span class="string">&quot;&lt;/strong&gt;\&quot;, 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; at char &quot;</span><span class="keyword">.</span><span class="default">xml_get_current_column_number</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser</span><span class="keyword">) . </span><span class="string">&quot; 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; on line &quot;</span><span class="keyword">.</span><span class="default">xml_get_current_line_number</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parser</span><span class="keyword">).</span><span class="string">&quot;&quot;</span><span class="keyword">;
<br>&#xA0; &#xA0; }else </span><span class="default">$thisError </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">parse_error</span><span class="keyword">;
<br>&#xA0; &#xA0; return </span><span class="default">$thisError</span><span class="keyword">;
<br>&#xA0; }
<br>&#xA0; 
<br>&#xA0; private function </span><span class="default">tag_open</span><span class="keyword">(</span><span class="default">$parser</span><span class="keyword">, </span><span class="default">$tag</span><span class="keyword">, </span><span class="default">$attributes</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">convert_to_array</span><span class="keyword">(</span><span class="default">$tag</span><span class="keyword">, </span><span class="string">&apos;attrib&apos;</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$idx</span><span class="keyword">=</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">convert_to_array</span><span class="keyword">(</span><span class="default">$tag</span><span class="keyword">, </span><span class="string">&apos;cdata&apos;</span><span class="keyword">); 
<br>&#xA0; &#xA0; if(isset(</span><span class="default">$idx</span><span class="keyword">)) { 
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">[</span><span class="default">$tag</span><span class="keyword">][</span><span class="default">$idx</span><span class="keyword">] = Array(</span><span class="string">&apos;@idx&apos; </span><span class="keyword">=&gt; </span><span class="default">$idx</span><span class="keyword">,</span><span class="string">&apos;@parent&apos; </span><span class="keyword">=&gt; &amp;</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer </span><span class="keyword">=&amp; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">[</span><span class="default">$tag</span><span class="keyword">][</span><span class="default">$idx</span><span class="keyword">];
<br>&#xA0; &#xA0; }else {
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">[</span><span class="default">$tag</span><span class="keyword">] = Array(</span><span class="string">&apos;@parent&apos; </span><span class="keyword">=&gt; &amp;</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer </span><span class="keyword">=&amp; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">[</span><span class="default">$tag</span><span class="keyword">];
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; if (!empty(</span><span class="default">$attributes</span><span class="keyword">)) { </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">[</span><span class="string">&apos;attrib&apos;</span><span class="keyword">] = </span><span class="default">$attributes</span><span class="keyword">; }
<br>&#xA0; }
<br>
<br>&#xA0; </span><span class="comment">/** Adds the current elements content to the current pointer[cdata] array. */
<br>&#xA0; </span><span class="keyword">private function </span><span class="default">cdata</span><span class="keyword">(</span><span class="default">$parser</span><span class="keyword">, </span><span class="default">$cdata</span><span class="keyword">) { </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">[</span><span class="string">&apos;cdata&apos;</span><span class="keyword">] = </span><span class="default">trim</span><span class="keyword">(</span><span class="default">$cdata</span><span class="keyword">); }
<br>
<br>&#xA0; private function </span><span class="default">tag_close</span><span class="keyword">(</span><span class="default">$parser</span><span class="keyword">, </span><span class="default">$tag</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">$current </span><span class="keyword">= &amp; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">;
<br>&#xA0; &#xA0; if(isset(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">[</span><span class="string">&apos;@idx&apos;</span><span class="keyword">])) {unset(</span><span class="default">$current</span><span class="keyword">[</span><span class="string">&apos;@idx&apos;</span><span class="keyword">]);}
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer </span><span class="keyword">= &amp; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">[</span><span class="string">&apos;@parent&apos;</span><span class="keyword">];
<br>&#xA0; &#xA0; unset(</span><span class="default">$current</span><span class="keyword">[</span><span class="string">&apos;@parent&apos;</span><span class="keyword">]);
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; if(isset(</span><span class="default">$current</span><span class="keyword">[</span><span class="string">&apos;cdata&apos;</span><span class="keyword">]) &amp;&amp; </span><span class="default">count</span><span class="keyword">(</span><span class="default">$current</span><span class="keyword">) == </span><span class="default">1</span><span class="keyword">) { </span><span class="default">$current </span><span class="keyword">= </span><span class="default">$current</span><span class="keyword">[</span><span class="string">&apos;cdata&apos;</span><span class="keyword">];}
<br>&#xA0; &#xA0; else if(empty(</span><span class="default">$current</span><span class="keyword">[</span><span class="string">&apos;cdata&apos;</span><span class="keyword">])) {unset(</span><span class="default">$current</span><span class="keyword">[</span><span class="string">&apos;cdata&apos;</span><span class="keyword">]);}
<br>&#xA0; }
<br>&#xA0; 
<br>&#xA0; </span><span class="comment">/** Converts a single element item into array(element[0]) if a second element of the same name is encountered. */
<br>&#xA0; </span><span class="keyword">private function </span><span class="default">convert_to_array</span><span class="keyword">(</span><span class="default">$tag</span><span class="keyword">, </span><span class="default">$item</span><span class="keyword">) { 
<br>&#xA0; &#xA0; if(isset(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">[</span><span class="default">$tag</span><span class="keyword">][</span><span class="default">$item</span><span class="keyword">])) { 
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$content </span><span class="keyword">= </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">[</span><span class="default">$tag</span><span class="keyword">];
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">[</span><span class="default">$tag</span><span class="keyword">] = array((</span><span class="default">0</span><span class="keyword">) =&gt; </span><span class="default">$content</span><span class="keyword">);
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$idx </span><span class="keyword">= </span><span class="default">1</span><span class="keyword">;
<br>&#xA0; &#xA0; }else if (isset(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">[</span><span class="default">$tag</span><span class="keyword">])) { 
<br>&#xA0; &#xA0; &#xA0; </span><span class="default">$idx </span><span class="keyword">= </span><span class="default">count</span><span class="keyword">(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">[</span><span class="default">$tag</span><span class="keyword">]); 
<br>&#xA0; &#xA0; &#xA0; if(!isset(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">[</span><span class="default">$tag</span><span class="keyword">][</span><span class="default">0</span><span class="keyword">])) { 
<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">[</span><span class="default">$tag</span><span class="keyword">] as </span><span class="default">$key </span><span class="keyword">=&gt; </span><span class="default">$value</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; unset(</span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">[</span><span class="default">$tag</span><span class="keyword">][</span><span class="default">$key</span><span class="keyword">]);
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$this</span><span class="keyword">-&gt;</span><span class="default">pointer</span><span class="keyword">[</span><span class="default">$tag</span><span class="keyword">][</span><span class="default">0</span><span class="keyword">][</span><span class="default">$key</span><span class="keyword">] = </span><span class="default">$value</span><span class="keyword">;
<br>&#xA0; &#xA0; }}}else </span><span class="default">$idx </span><span class="keyword">= </span><span class="default">null</span><span class="keyword">;
<br>&#xA0; &#xA0; return </span><span class="default">$idx</span><span class="keyword">;
<br>&#xA0; }
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>This is supplimental information for the &quot;class xmlToArrayParser&quot;.
<br>This is a fully functional error free, extensively tested php class unlike the posts that follow it.
<br>
<br>Key phrase: Fully functional, fully tested, error free XML To Array parser.
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">/**
<br> * class xmlToArrayParser
<br> * 
<br>&#xA0; Notes: 
<br>&#xA0; 1. &apos;attrib&apos; and &apos;cdata&apos; are keys added to the array when the element contains both attributes and content.
<br>&#xA0; 2. Ignores content that is not in between it&apos;s own set of tags.
<br>&#xA0; 3. Don&apos;t know if it recognizes processing instructions nor do I know about processing instructions.
<br>&#xA0; &#xA0;&#xA0; &lt;\?some_pi some_attr=&quot;some_value&quot;?&gt;&#xA0; This is the same as a document declaration.
<br>&#xA0; 4. Empty elements are not included unless they have attributes.
<br>&#xA0; 5. Version 2.0, Dec. 2, 2011, added xml error reporting.
<br>&#xA0; 
<br>&#xA0; Usage:
<br>&#xA0; &#xA0; $domObj = new xmlToArrayParser($xml);
<br>&#xA0; &#xA0; $elemVal = $domObj-&gt;array[&apos;element&apos;]
<br>&#xA0; &#xA0; Or assign the entire array to its own variable:
<br>&#xA0; &#xA0; $domArr = $domObj-&gt;array;
<br>&#xA0; &#xA0; $elemVal = $domArr[&apos;element&apos;]
<br>&#xA0; 
<br>&#xA0; Example:
<br>&#xA0; &#xA0; $xml = &apos;&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;no&quot;?&gt;
<br>&#xA0; &#xA0; &lt;top&gt;
<br>&#xA0; &#xA0; &#xA0; &lt;element1&gt;element content 1&lt;/element1&gt;
<br>&#xA0; &#xA0; &#xA0; &lt;element2 var2=&quot;val2&quot; /&gt;
<br>&#xA0; &#xA0; &#xA0; &lt;element3 var3=&quot;val3&quot; var4=&quot;val4&quot;&gt;element content 3&lt;/element3&gt; 
<br>&#xA0; &#xA0; &#xA0; &lt;element3 var5=&quot;val5&quot;&gt;element content 4&lt;/element3&gt;
<br>&#xA0; &#xA0; &#xA0; &lt;element3 var6=&quot;val6&quot; /&gt;
<br>&#xA0; &#xA0; &#xA0; &lt;element3&gt;element content 7&lt;/element3&gt;
<br>&#xA0; &#xA0; &lt;/top&gt;&apos;;
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; $domObj = new xmlToArrayParser($xml);
<br>&#xA0; &#xA0; $domArr = $domObj-&gt;array;
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; if($domObj-&gt;parse_error) echo $domObj-&gt;get_xml_error();
<br>&#xA0; &#xA0; else print_r($domArr);
<br>
<br>&#xA0; &#xA0; On Success:
<br>&#xA0; &#xA0; $domArr[&apos;top&apos;][&apos;element1&apos;] =&gt; element content 1
<br>&#xA0; &#xA0; $domArr[&apos;top&apos;][&apos;element2&apos;][&apos;attrib&apos;][&apos;var2&apos;] =&gt; val2
<br>&#xA0; &#xA0; $domArr[&apos;top&apos;][&apos;element3&apos;][&apos;0&apos;][&apos;attrib&apos;][&apos;var3&apos;] =&gt; val3
<br>&#xA0; &#xA0; $domArr[&apos;top&apos;][&apos;element3&apos;][&apos;0&apos;][&apos;attrib&apos;][&apos;var4&apos;] =&gt; val4
<br>&#xA0; &#xA0; $domArr[&apos;top&apos;][&apos;element3&apos;][&apos;0&apos;][&apos;cdata&apos;] =&gt; element content 3
<br>&#xA0; &#xA0; $domArr[&apos;top&apos;][&apos;element3&apos;][&apos;1&apos;][&apos;attrib&apos;][&apos;var5&apos;] =&gt; val5
<br>&#xA0; &#xA0; $domArr[&apos;top&apos;][&apos;element3&apos;][&apos;1&apos;][&apos;cdata&apos;] =&gt; element content 4
<br>&#xA0; &#xA0; $domArr[&apos;top&apos;][&apos;element3&apos;][&apos;2&apos;][&apos;attrib&apos;][&apos;var6&apos;] =&gt; val6
<br>&#xA0; &#xA0; $domArr[&apos;top&apos;][&apos;element3&apos;][&apos;3&apos;] =&gt; element content 7
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; On Error:
<br>&#xA0; &#xA0; Error Code [76] &quot;Mismatched tag&quot;, at char 58 on line 3
<br> *
<br> */
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/ref.xml.php)

**[To root](/README.md)**