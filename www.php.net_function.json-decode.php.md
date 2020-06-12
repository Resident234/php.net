# json_decode




<div class="phpcode"><span class="html">
Make sure you pass in utf8 content, or json_decode may error out and just return a null value.&#xA0; For a particular web service I was using, I had to do the following:
<br>
<br><span class="default">&lt;?php
<br>$contents </span><span class="keyword">= </span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="default">$url</span><span class="keyword">);
<br></span><span class="default">$contents </span><span class="keyword">= </span><span class="default">utf8_encode</span><span class="keyword">(</span><span class="default">$contents</span><span class="keyword">);
<br></span><span class="default">$results </span><span class="keyword">= </span><span class="default">json_decode</span><span class="keyword">(</span><span class="default">$contents</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Hope this helps!</span>
</div>
  

#


<div class="phpcode"><span class="html">
First of all, since JSON is not native PHP format or even native JavaScript format, everyone who wants to use JSON wisely should carefuly read official documentation. There is a link to it here, in &quot;Introduction&quot; section. Many questions like &quot;it doesn&apos;t recognize my strings&quot; and those like previous one (about zip codes) will drop if you will be attentive!
<br>
<br>And second. I&apos;ve found that there is no good, real working example of how to validate string if it is a JSON or not.
<br>There are two ways to make this: parse input string for yourself using regular expressions or anything else, use json_decode to do it for you.
<br>Parsing for yourself is like writing your own compiler, too difficult.
<br>Just testing result of json_decode is not enough because you should test it with NULL, but valid JSON could be like this &apos;null&apos; and it will evaluate to NULL. So you should use another function - json_last_error. It will return error code of the last encode/decode operation. If no error occured it will be JSON_ERROR_NONE. So here is the function you should use for testing:
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">isValidJson</span><span class="keyword">(</span><span class="default">$strJson</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="default">json_decode</span><span class="keyword">(</span><span class="default">$strJson</span><span class="keyword">);
<br>&#xA0; &#xA0; return (</span><span class="default">json_last_error</span><span class="keyword">() === </span><span class="default">JSON_ERROR_NONE</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;
<br></span>It&apos;s so simple, that there is no need to use it and slow down your script with extra delay for function call. Just do it manualy in you code while working with input data:
<br><span class="default">&lt;?php
<br></span><span class="comment">//here is my initial string
<br></span><span class="default">$sJson </span><span class="keyword">= </span><span class="default">$_POST</span><span class="keyword">[</span><span class="string">&apos;json&apos;</span><span class="keyword">];
<br></span><span class="comment">//try to decode it
<br></span><span class="default">$json </span><span class="keyword">= </span><span class="default">json_decode</span><span class="keyword">(</span><span class="default">$sJson</span><span class="keyword">);
<br>if (</span><span class="default">json_last_error</span><span class="keyword">() === </span><span class="default">JSON_ERROR_NONE</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="comment">//do something with $json. It&apos;s ready to use
<br></span><span class="keyword">} else {
<br>&#xA0; &#xA0; </span><span class="comment">//yep, it&apos;s not JSON. Log error or alert someone or do nothing
<br></span><span class="keyword">}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
The function by 1franck to allow comments works except if there is a comment at the very beginning of the file. Here&apos;s a modified version (only the regex was changed) that accounts for that.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">json_clean_decode</span><span class="keyword">(</span><span class="default">$json</span><span class="keyword">, </span><span class="default">$assoc </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">, </span><span class="default">$depth </span><span class="keyword">= </span><span class="default">512</span><span class="keyword">, </span><span class="default">$options </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="comment">// search and remove comments like /* */ and //<br>&#xA0; &#xA0; </span><span class="default">$json </span><span class="keyword">= </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&quot;#(/\*([^*]|[\r\n]|(\*+([^*/]|[\r\n])))*\*+/)|([\s\t]//.*)|(^//.*)#&quot;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="default">$json</span><span class="keyword">);<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; if(</span><span class="default">version_compare</span><span class="keyword">(</span><span class="default">phpversion</span><span class="keyword">(), </span><span class="string">&apos;5.4.0&apos;</span><span class="keyword">, </span><span class="string">&apos;&gt;=&apos;</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$json </span><span class="keyword">= </span><span class="default">json_decode</span><span class="keyword">(</span><span class="default">$json</span><span class="keyword">, </span><span class="default">$assoc</span><span class="keyword">, </span><span class="default">$depth</span><span class="keyword">, </span><span class="default">$options</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; elseif(</span><span class="default">version_compare</span><span class="keyword">(</span><span class="default">phpversion</span><span class="keyword">(), </span><span class="string">&apos;5.3.0&apos;</span><span class="keyword">, </span><span class="string">&apos;&gt;=&apos;</span><span class="keyword">)) {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$json </span><span class="keyword">= </span><span class="default">json_decode</span><span class="keyword">(</span><span class="default">$json</span><span class="keyword">, </span><span class="default">$assoc</span><span class="keyword">, </span><span class="default">$depth</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; else {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$json </span><span class="keyword">= </span><span class="default">json_decode</span><span class="keyword">(</span><span class="default">$json</span><span class="keyword">, </span><span class="default">$assoc</span><span class="keyword">);<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; return </span><span class="default">$json</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
When decoding strings from the database, make sure the input was encoded with the correct charset when it was input to the database.<br><br>I was using a form to create records in the DB which had a content field that was valid JSON, but it included curly apostrophes.&#xA0; If the page with the form did not have <br><br>&lt;meta http-equiv=&quot;Content-Type&quot; content=&quot;text/html;charset=utf-8&quot;&gt;<br><br>in the head, then the data was sent to the database with the wrong encoding.&#xA0; Then, when json_decode tried to convert the string to an object, it failed every time.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Sometime, i need to allow comments in json file. So i wrote a small func to clean comments in a json string before decoding it:
<br><span class="default">&lt;?php
<br></span><span class="comment">/**
<br> * Clean comments of json content and decode it with json_decode().
<br> * Work like the original php json_decode() function with the same params
<br> *
<br> * @param&#xA0;&#xA0; string&#xA0; $json&#xA0; &#xA0; The json string being decoded
<br> * @param&#xA0;&#xA0; bool&#xA0; &#xA0; $assoc&#xA0;&#xA0; When TRUE, returned objects will be converted into associative arrays. 
<br> * @param&#xA0;&#xA0; integer $depth&#xA0;&#xA0; User specified recursion depth. (&gt;=5.3)
<br> * @param&#xA0;&#xA0; integer $options Bitmask of JSON decode options. (&gt;=5.4)
<br> * @return&#xA0; string
<br> */
<br></span><span class="keyword">function </span><span class="default">json_clean_decode</span><span class="keyword">(</span><span class="default">$json</span><span class="keyword">, </span><span class="default">$assoc </span><span class="keyword">= </span><span class="default">false</span><span class="keyword">, </span><span class="default">$depth </span><span class="keyword">= </span><span class="default">512</span><span class="keyword">, </span><span class="default">$options </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">) {
<br>
<br>&#xA0; &#xA0; </span><span class="comment">// search and remove comments like /* */ and //
<br>&#xA0; &#xA0; </span><span class="default">$json </span><span class="keyword">= </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&quot;#(/\*([^*]|[\r\n]|(\*+([^*/]|[\r\n])))*\*+/)|([\s\t](//).*)#&quot;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="default">$json</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; if(</span><span class="default">version_compare</span><span class="keyword">(</span><span class="default">phpversion</span><span class="keyword">(), </span><span class="string">&apos;5.4.0&apos;</span><span class="keyword">, </span><span class="string">&apos;&gt;=&apos;</span><span class="keyword">)) { 
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$json </span><span class="keyword">= </span><span class="default">json_decode</span><span class="keyword">(</span><span class="default">$json</span><span class="keyword">, </span><span class="default">$assoc</span><span class="keyword">, </span><span class="default">$depth</span><span class="keyword">, </span><span class="default">$options</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; elseif(</span><span class="default">version_compare</span><span class="keyword">(</span><span class="default">phpversion</span><span class="keyword">(), </span><span class="string">&apos;5.3.0&apos;</span><span class="keyword">, </span><span class="string">&apos;&gt;=&apos;</span><span class="keyword">)) { 
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$json </span><span class="keyword">= </span><span class="default">json_decode</span><span class="keyword">(</span><span class="default">$json</span><span class="keyword">, </span><span class="default">$assoc</span><span class="keyword">, </span><span class="default">$depth</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$json </span><span class="keyword">= </span><span class="default">json_decode</span><span class="keyword">(</span><span class="default">$json</span><span class="keyword">, </span><span class="default">$assoc</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>
<br>&#xA0; &#xA0; return </span><span class="default">$json</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
it seems, that some of the people are not aware, that if you are using json_decode to decode a string it HAS to be a propper json string:<br><br><span class="default">&lt;?php<br>var_dump</span><span class="keyword">(</span><span class="default">json_encode</span><span class="keyword">(</span><span class="string">&apos;Hello&apos;</span><span class="keyword">));<br><br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">json_decode</span><span class="keyword">(</span><span class="string">&apos;Hello&apos;</span><span class="keyword">));&#xA0; </span><span class="comment">// wrong<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">json_decode</span><span class="keyword">(</span><span class="string">&quot;Hello&quot;</span><span class="keyword">)); </span><span class="comment">// wrong<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">json_decode</span><span class="keyword">(</span><span class="string">&apos;&quot;Hello&quot;&apos;</span><span class="keyword">)); </span><span class="comment">// correct<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">json_decode</span><span class="keyword">(</span><span class="string">&quot;&apos;Hello&apos;&quot;</span><span class="keyword">)); </span><span class="comment">// wrong<br><br></span><span class="default">result</span><span class="keyword">:<br><br></span><span class="default">string</span><span class="keyword">(</span><span class="default">7</span><span class="keyword">) </span><span class="string">&quot;&quot;</span><span class="default">Hello</span><span class="string">&quot;&quot;<br></span><span class="default">NULL<br>NULL<br>string</span><span class="keyword">(</span><span class="default">5</span><span class="keyword">) </span><span class="string">&quot;Hello&quot;<br></span><span class="default">NULL</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If var_dump produces NULL, you may be experiencing JSONP aka JSON with padding, here&apos;s a quick fix...
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="comment">//remove padding
<br></span><span class="default">$body</span><span class="keyword">=</span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/.+?({.+}).+/&apos;</span><span class="keyword">,</span><span class="string">&apos;$1&apos;</span><span class="keyword">,</span><span class="default">$body</span><span class="keyword">);
<br>
<br></span><span class="comment">// now, process the JSON string
<br></span><span class="default">$result </span><span class="keyword">= </span><span class="default">json_decode</span><span class="keyword">(</span><span class="default">$body</span><span class="keyword">);
<br>
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$result</span><span class="keyword">);
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be aware, when decoding JSON strings, where an empty string is a key, this library replaces the empty string with &quot;_empty_&quot;.<br><br>So the following code gives an unexpected result:<br><span class="default">&lt;?php<br>var_dump</span><span class="keyword">(</span><span class="default">json_decode</span><span class="keyword">(</span><span class="string">&apos;{&quot;&quot;:&quot;arbitrary&quot;}&apos;</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>The result is as follows:<br>object(stdClass)#1 (1) {<br>&#xA0; [&quot;_empty_&quot;]=&gt;<br>&#xA0; string(6) &quot;arbitrary&quot;<br>}<br><br>Any subsequent key named &quot;_empty_&quot; (or &quot;&quot; [the empty string] again) will overwrite the value.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I added a 3rd regex to the json_decode_nice function by &quot;colin.mollenhour.com&quot; to handle a trailing comma in json definition.<br><br><span class="default">&lt;?php<br></span><span class="comment">// <a href="http://www.php.net/manual/en/function.json-decode.php#95782" rel="nofollow" target="_blank">http://www.php.net/manual/en/function.json-decode.php#95782</a><br></span><span class="keyword">function </span><span class="default">json_decode_nice</span><span class="keyword">(</span><span class="default">$json</span><span class="keyword">, </span><span class="default">$assoc </span><span class="keyword">= </span><span class="default">FALSE</span><span class="keyword">){ <br>&#xA0; &#xA0; </span><span class="default">$json </span><span class="keyword">= </span><span class="default">str_replace</span><span class="keyword">(array(</span><span class="string">&quot;\n&quot;</span><span class="keyword">,</span><span class="string">&quot;\r&quot;</span><span class="keyword">),</span><span class="string">&quot;&quot;</span><span class="keyword">,</span><span class="default">$json</span><span class="keyword">); <br>&#xA0; &#xA0; </span><span class="default">$json </span><span class="keyword">= </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/([{,]+)(\s*)([^&quot;]+?)\s*:/&apos;</span><span class="keyword">,</span><span class="string">&apos;$1&quot;$3&quot;:&apos;</span><span class="keyword">,</span><span class="default">$json</span><span class="keyword">);<br>&#xA0; &#xA0; </span><span class="default">$json </span><span class="keyword">= </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/(,)\s*}$/&apos;</span><span class="keyword">,</span><span class="string">&apos;}&apos;</span><span class="keyword">,</span><span class="default">$json</span><span class="keyword">);<br>&#xA0; &#xA0; return </span><span class="default">json_decode</span><span class="keyword">(</span><span class="default">$json</span><span class="keyword">,</span><span class="default">$assoc</span><span class="keyword">); <br>}<br></span><span class="default">?&gt;<br></span><br>Example:<br><br><span class="default">&lt;?php<br>$dat_json </span><span class="keyword">= &lt;&lt;&lt;EOF<br></span><span class="string">{<br>&#xA0; &#xA0; &quot;foo&quot;&#xA0; &#xA0; : &quot;bam&quot;,<br>&#xA0; &#xA0; &quot;bar&quot;&#xA0; &#xA0; : &quot;baz&quot;,<br>}<br></span><span class="keyword">EOF;<br></span><span class="default">$dat_array </span><span class="keyword">= </span><span class="default">json_decode_nice</span><span class="keyword">( </span><span class="default">$dat_json </span><span class="keyword">);<br></span><span class="default">var_dump </span><span class="keyword">( </span><span class="default">$dat_json</span><span class="keyword">, </span><span class="default">$dat_array </span><span class="keyword">); <br><br></span><span class="comment">/* RESULTS:<br><br>string(35) &quot;{<br>&#xA0; &#xA0; &quot;foo&quot;&#xA0; &#xA0; : &quot;bam&quot;,<br>&#xA0; &#xA0; &quot;bar&quot;&#xA0; &#xA0; : &quot;baz&quot;,<br>}&quot;<br>array(2) {<br>&#xA0; [&quot;foo&quot;]=&gt;<br>&#xA0; string(3) &quot;bam&quot;<br>&#xA0; [&quot;bar&quot;]=&gt;<br>&#xA0; string(3) &quot;baz&quot;<br>}<br>*/<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
json_decode_nice + keep linebreaks:<br><br>function json_decode_nice($json, $assoc = TRUE){<br>&#xA0; &#xA0; $json = str_replace(array(&quot;\n&quot;,&quot;\r&quot;),&quot;\\n&quot;,$json);<br>&#xA0; &#xA0; $json = preg_replace(&apos;/([{,]+)(\s*)([^&quot;]+?)\s*:/&apos;,&apos;$1&quot;$3&quot;:&apos;,$json);<br>&#xA0; &#xA0; $json = preg_replace(&apos;/(,)\s*}$/&apos;,&apos;}&apos;,$json);<br>&#xA0; &#xA0; return json_decode($json,$assoc);<br>}<br><br>by phpdoc at badassawesome dot com, I just changed line 2.<br>If you want to keep the linebreaks just escape the slash.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Noted in a comment below is that this function will return NULL when given a simple string.<br><br>This is new behavior - see the result in PHP 5.2.4 :<br>php &gt; var_dump(json_decode(&apos;this is a simple string&apos;));<br>string(23) &quot;this is a simple string&quot;<br><br>in PHP 5.3.2 :<br>php &gt; var_dump(json_decode(&apos;this is a simple string&apos;));<br>NULL<br><br>I had several functions that relied on checking the value of a purported JSON string if it didn&apos;t decode into an object/array. If you do too, be sure to be aware of this when upgrading to PHP 5.3.</span>
</div>
  

#


<div class="phpcode"><span class="html">
This function will remove trailing commas and encode in utf8, which might solve many people&apos;s problems. Someone might want to expand it to also change single quotes to double quotes, and fix other kinds of json breakage.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">mjson_decode</span><span class="keyword">(</span><span class="default">$json</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">json_decode</span><span class="keyword">(</span><span class="default">removeTrailingCommas</span><span class="keyword">(</span><span class="default">utf8_encode</span><span class="keyword">(</span><span class="default">$json</span><span class="keyword">)));<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; function </span><span class="default">removeTrailingCommas</span><span class="keyword">(</span><span class="default">$json</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$json</span><span class="keyword">=</span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/,\s*([\]}])/m&apos;</span><span class="keyword">, </span><span class="string">&apos;$1&apos;</span><span class="keyword">, </span><span class="default">$json</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$json</span><span class="keyword">;<br>&#xA0; &#xA0; }<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
For those of you wanting json_decode to be a little more lenient (more like Javascript), here is a wrapper:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">json_decode_nice</span><span class="keyword">(</span><span class="default">$json</span><span class="keyword">, </span><span class="default">$assoc </span><span class="keyword">= </span><span class="default">FALSE</span><span class="keyword">){
<br>&#xA0; &#xA0; </span><span class="default">$json </span><span class="keyword">= </span><span class="default">str_replace</span><span class="keyword">(array(</span><span class="string">&quot;\n&quot;</span><span class="keyword">,</span><span class="string">&quot;\r&quot;</span><span class="keyword">),</span><span class="string">&quot;&quot;</span><span class="keyword">,</span><span class="default">$json</span><span class="keyword">);
<br>&#xA0; &#xA0; </span><span class="default">$json </span><span class="keyword">= </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&apos;/([{,]+)(\s*)([^&quot;]+?)\s*:/&apos;</span><span class="keyword">,</span><span class="string">&apos;$1&quot;$3&quot;:&apos;</span><span class="keyword">,</span><span class="default">$json</span><span class="keyword">);
<br>&#xA0; &#xA0; return </span><span class="default">json_decode</span><span class="keyword">(</span><span class="default">$json</span><span class="keyword">,</span><span class="default">$assoc</span><span class="keyword">);
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>Some examples of accepted syntax:
<br>
<br><span class="default">&lt;?php
<br>$json </span><span class="keyword">= </span><span class="string">&apos;{a:{b:&quot;c&quot;,d:[&quot;e&quot;,&quot;f&quot;,0]}}&apos;</span><span class="keyword">;
<br></span><span class="default">$json </span><span class="keyword">= 
<br></span><span class="string">&apos;{
<br>&#xA0;&#xA0; a : {
<br>&#xA0; &#xA0; &#xA0; b : &quot;c&quot;,
<br>&#xA0; &#xA0; &#xA0; &quot;d.e.f&quot;: &quot;g&quot;
<br>&#xA0;&#xA0; }
<br>}&apos;</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>If your content needs to have newlines, do this:
<br>
<br><span class="default">&lt;?php
<br>$string </span><span class="keyword">= </span><span class="string">&quot;This
<br>Text
<br>Has
<br>Newlines&quot;</span><span class="keyword">;
<br></span><span class="default">$json </span><span class="keyword">= </span><span class="string">&apos;{withnewlines:&apos;</span><span class="keyword">.</span><span class="default">json_encode</span><span class="keyword">(</span><span class="default">$string</span><span class="keyword">).</span><span class="string">&apos;}&apos;</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>Note: This does not fix trailing commas or single quotes.
<br>
<br>[EDIT BY danbrown AT php DOT net: Contains a bugfix provided by (sskaje AT gmail DOT com) on 05-DEC-2012 with the following note.]
<br>
<br>Old regexp failed when json like
<br>{aaa:[{a:1},{a:2}]}</span>
</div>
  

#


<div class="phpcode"><span class="html">
json_decode()&apos;s handling of invalid JSON is very flaky, and it is very hard to reliably determine if the decoding succeeded or not. Observe the following examples, none of which contain valid JSON:
<br>
<br>The following each returns NULL, as you might expect:
<br>
<br><span class="default">&lt;?php
<br>var_dump</span><span class="keyword">(</span><span class="default">json_decode</span><span class="keyword">(</span><span class="string">&apos;[&apos;</span><span class="keyword">));&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// unmatched bracket
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">json_decode</span><span class="keyword">(</span><span class="string">&apos;{&apos;</span><span class="keyword">));&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// unmatched brace
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">json_decode</span><span class="keyword">(</span><span class="string">&apos;{}}&apos;</span><span class="keyword">));&#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// unmatched brace
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">json_decode</span><span class="keyword">(</span><span class="string">&apos;{error error}&apos;</span><span class="keyword">)); </span><span class="comment">// invalid object key/value
<br></span><span class="default">notation
<br>var_dump</span><span class="keyword">(</span><span class="default">json_decode</span><span class="keyword">(</span><span class="string">&apos;[&quot;\&quot;]&apos;</span><span class="keyword">));&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="comment">// unclosed string
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">json_decode</span><span class="keyword">(</span><span class="string">&apos;[&quot; \x &quot;]&apos;</span><span class="keyword">));&#xA0; &#xA0; &#xA0; </span><span class="comment">// invalid escape code
<br>
<br></span><span class="default">Yet the following each returns the literal string you passed to it</span><span class="keyword">:
<br>
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">json_decode</span><span class="keyword">(</span><span class="string">&apos; [&apos;</span><span class="keyword">)); </span><span class="comment">// unmatched bracket
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">json_decode</span><span class="keyword">(</span><span class="string">&apos; {&apos;</span><span class="keyword">)); </span><span class="comment">// unmatched brace
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">json_decode</span><span class="keyword">(</span><span class="string">&apos; {}}&apos;</span><span class="keyword">)); </span><span class="comment">// unmatched brace
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">json_decode</span><span class="keyword">(</span><span class="string">&apos; {error error}&apos;</span><span class="keyword">)); </span><span class="comment">// invalid object key/value notation
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">json_decode</span><span class="keyword">(</span><span class="string">&apos;&quot;\&quot;&apos;</span><span class="keyword">)); </span><span class="comment">// unclosed string
<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">json_decode</span><span class="keyword">(</span><span class="string">&apos;&quot; \x &quot;&apos;</span><span class="keyword">)); </span><span class="comment">// invalid escape code
<br></span><span class="default">?&gt;
<br></span>
<br>(this is on PHP 5.2.6)
<br>
<br>Reported as a bug, but oddly enough, it was closed as not a bug.
<br>
<br>[NOTE BY danbrown AT php DOT net: This was later re-evaluated and it was determined that an issue did in fact exist, and was patched by members of the Development Team.&#xA0; See <a href="http://bugs.php.net/bug.php?id=45989" rel="nofollow" target="_blank">http://bugs.php.net/bug.php?id=45989</a> for details.]</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.json-decode.php)

**[⬆ to root](/)**