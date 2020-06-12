# preg_match




<div class="phpcode"><span class="html">
Simple regex<br><br>Regex quick reference<br>[abc]&#xA0; &#xA0;&#xA0; A single character: a, b or c<br>[^abc]&#xA0; &#xA0;&#xA0; Any single character but a, b, or c<br>[a-z]&#xA0; &#xA0;&#xA0; Any single character in the range a-z<br>[a-zA-Z]&#xA0; &#xA0;&#xA0; Any single character in the range a-z or A-Z<br>^&#xA0; &#xA0;&#xA0; Start of line<br>$&#xA0; &#xA0;&#xA0; End of line<br>\A&#xA0; &#xA0;&#xA0; Start of string<br>\z&#xA0; &#xA0;&#xA0; End of string<br>.&#xA0; &#xA0;&#xA0; Any single character<br>\s&#xA0; &#xA0;&#xA0; Any whitespace character<br>\S&#xA0; &#xA0;&#xA0; Any non-whitespace character<br>\d&#xA0; &#xA0;&#xA0; Any digit<br>\D&#xA0; &#xA0;&#xA0; Any non-digit<br>\w&#xA0; &#xA0;&#xA0; Any word character (letter, number, underscore)<br>\W&#xA0; &#xA0;&#xA0; Any non-word character<br>\b&#xA0; &#xA0;&#xA0; Any word boundary character<br>(...)&#xA0; &#xA0;&#xA0; Capture everything enclosed<br>(a|b)&#xA0; &#xA0;&#xA0; a or b<br>a?&#xA0; &#xA0;&#xA0; Zero or one of a<br>a*&#xA0; &#xA0;&#xA0; Zero or more of a<br>a+&#xA0; &#xA0;&#xA0; One or more of a<br>a{3}&#xA0; &#xA0;&#xA0; Exactly 3 of a<br>a{3,}&#xA0; &#xA0;&#xA0; 3 or more of a<br>a{3,6}&#xA0; &#xA0;&#xA0; Between 3 and 6 of a<br><br>options: i case insensitive m make dot match newlines x ignore whitespace in regex o perform #{...} substitutions only once</span>
</div>
  

#


<div class="phpcode"><span class="html">
Was working on a site that needed japanese and alphabetic letters and needed to <br>validate input using preg_match, I tried using \p{script} but didn&apos;t work:<br><br><span class="default">&lt;?php<br>$pattern </span><span class="keyword">=</span><span class="string">&apos;/^([-a-zA-Z0-9_\p{Katakana}\p{Hiragana}\p{Han}]*)$/u&apos;</span><span class="keyword">; </span><span class="comment">// Didn&apos;t work<br></span><span class="default">?&gt;<br></span><br>So I tried with ranges and it worked:<br><br><span class="default">&lt;?php<br>$pattern </span><span class="keyword">=</span><span class="string">&apos;/^[-a-zA-Z0-9_\x{30A0}-\x{30FF}&apos;<br>&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">.</span><span class="string">&apos;\x{3040}-\x{309F}\x{4E00}-\x{9FBF}\s]*$/u&apos;</span><span class="keyword">;<br></span><span class="default">$match_string </span><span class="keyword">= </span><span class="string">&apos;&#x5370;&#x5237;&#x6700;&#x5B89; &#x30CB;&#x30AD;&#x30D3;&#x8DE1;&#x9664;&#x53BB; &#x30B2;&#x30FC;&#x30E0;&#x30DC;&#x30FC;&#x30A4;&apos;</span><span class="keyword">;<br><br>if (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="default">$pattern</span><span class="keyword">, </span><span class="default">$match_string</span><span class="keyword">)) {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;Found - pattern </span><span class="default">$pattern</span><span class="string">&quot;</span><span class="keyword">;<br>} else {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;Not found - pattern </span><span class="default">$pattern</span><span class="string">&quot;</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>U+4E00&#x2013;U+9FBF Kanji<br>U+3040&#x2013;U+309F Hiragana<br>U+30A0&#x2013;U+30FF Katakana<br><br>Hope its useful, it took me several hours to figure it out.</span>
</div>
  

#


<div class="phpcode"><span class="html">
I noticed that in order to deal with UTF-8 texts, without having to recompile php with the PCRE UTF-8 flag enabled, you can just add the following sequence at the start of your pattern: (*UTF8)<br><br>for instance : &apos;#(*UTF8)[[:alnum:]]#&apos; will return TRUE for &apos;&#xE9;&apos; where &apos;#[[:alnum:]]#&apos; will return FALSE<br><br>found this very very useful tip after hours of research over the web directly in pcre website right here : <a href="http://www.pcre.org/pcre.txt" rel="nofollow" target="_blank">http://www.pcre.org/pcre.txt</a><br>there are many further informations about UTF-8 support in the lib<br><br>hop that will help!<br><br>--<br>cedric</span>
</div>
  

#


<div class="phpcode"><span class="html">
Sometimes its useful to negate a string. The first method which comes to mind to do this is: [^(string)] but this of course won&apos;t work. There is a solution, but it is not very well known. This is the simple piece of code on how a negation of a string is done:<br><br>(?:(?!string).)<br><br>?: makes a subpattern (see <a href="http://www.php.net/manual/en/regexp.reference.subpatterns.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/regexp.reference.subpatterns.php</a>) and ?! is a negative look ahead. You put the negative look ahead in front of the dot because you want the regex engine to first check if there is an occurrence of the string you are negating. Only if it is not there, you want to match an arbitrary character.<br><br>Hope this helps some ppl.</span>
</div>
  

#


<div class="phpcode"><span class="html">
This sample regexp may be useful if you are working with DB field types. <br><br>(?P&lt;type&gt;\w+)($|\((?P&lt;length&gt;(\d+|(.*)))\))<br><br>For example, if you are have a such type as &quot;varchar(255)&quot; or &quot;text&quot;, the next fragment<br><br><span class="default">&lt;?php<br>&#xA0;&#xA0; $type </span><span class="keyword">= </span><span class="string">&apos;varchar(255)&apos;</span><span class="keyword">;&#xA0; </span><span class="comment">// type of field<br>&#xA0;&#xA0; </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/(?P&lt;type&gt;\w+)($|\((?P&lt;length&gt;(\d+|(.*)))\))/&apos;</span><span class="keyword">, </span><span class="default">$type</span><span class="keyword">, </span><span class="default">$field</span><span class="keyword">);<br>&#xA0;&#xA0; </span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$field</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>will output something like this:<br>Array ( [0] =&gt; varchar(255) [type] =&gt; varchar [1] =&gt; varchar [2] =&gt; (255) [length] =&gt; 255 [3] =&gt; 255 [4] =&gt; 255 )</span>
</div>
  

#


<div class="phpcode"><span class="html">
I just learned about named groups from a Python friend today and was curious if PHP supported them, guess what -- it does!!!<br><br><a href="http://www.regular-expressions.info/named.html" rel="nofollow" target="_blank">http://www.regular-expressions.info/named.html</a><br><br><span class="default">&lt;?php<br>&#xA0;&#xA0; preg_match</span><span class="keyword">(</span><span class="string">&quot;/(?P&lt;foo&gt;abc)(.*)(?P&lt;bar&gt;xyz)/&quot;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="string">&apos;abcdefghijklmnopqrstuvwxyz&apos;</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">$matches</span><span class="keyword">);<br>&#xA0;&#xA0; </span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$matches</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>will produce: <br><br>Array<br>(<br>&#xA0; &#xA0; [0] =&gt; abcdefghijklmnopqrstuvwxyz<br>&#xA0; &#xA0; [foo] =&gt; abc<br>&#xA0; &#xA0; [1] =&gt; abc<br>&#xA0; &#xA0; [2] =&gt; defghijklmnopqrstuvw<br>&#xA0; &#xA0; [bar] =&gt; xyz<br>&#xA0; &#xA0; [3] =&gt; xyz<br>)<br><br>Note that you actually get the named group as well as the numerical key<br>value too, so if you do use them, and you&apos;re counting array elements, be<br>aware that your array might be bigger than you initially expect it to be.</span>
</div>
  

#


<div class="phpcode"><span class="html">
For those who search for a unicode regular expression example using preg_match here it is:<br><br>Check for Persian digits<br>preg_match( &quot;/[^\x{06F0}-\x{06F9}\x]+/u&quot; , &apos;&#x6F1;&#x6F2;&#x6F3;&#x6F4;&#x6F5;&#x6F6;&#x6F7;&#x6F8;&#x6F9;&#x6F0;&apos; );</span>
</div>
  

#


<div class="phpcode"><span class="html">
Matching a backslash character can be confusing, because double escaping is needed in the pattern: first for PHP, second for the regex engine<br><span class="default">&lt;?php<br></span><span class="comment">//match newline control character:<br></span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/\n/&apos;</span><span class="keyword">,</span><span class="string">&apos;\n&apos;</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">//pattern matches and is stored as control character 0x0A in the pattern string<br></span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/\\\n/&apos;</span><span class="keyword">,</span><span class="string">&apos;\n&apos;</span><span class="keyword">); </span><span class="comment">//very same match, but is stored escaped as 0x5C,0x6E in the pattern string<br><br>//trying to match &quot;\&apos;&quot; (2 characters) in a text file, &apos;\\\&apos;&apos; as PHP string:<br></span><span class="default">$subject </span><span class="keyword">= </span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="string">&apos;myfile.txt&apos;</span><span class="keyword">);<br></span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/\\\&apos;/&apos;</span><span class="keyword">,</span><span class="default">$subject</span><span class="keyword">);&#xA0; &#xA0; </span><span class="comment">//DOESN&apos;T MATCH!!! stored as 0x5C,0x27 (escaped apostrophe), this only matches apostrophe<br></span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/\\\\\&apos;/&apos;</span><span class="keyword">,</span><span class="default">$subject</span><span class="keyword">);&#xA0; </span><span class="comment">//matches, stored as 0x5C,0x5C,0x27 (escaped backslash and unescaped apostrophe)<br></span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/\\\\\\\/&apos;</span><span class="keyword">,</span><span class="default">$subject</span><span class="keyword">); </span><span class="comment">//also matches, stored as 0x5C,0x5C,0x5C,0x27 (escaped backslash and escaped apostrophe)<br><br>//matching &quot;\n&quot; (2 characters):<br></span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/\\\\n/&apos;</span><span class="keyword">,</span><span class="string">&apos;\\n&apos;</span><span class="keyword">);<br></span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/\\\n/&apos;</span><span class="keyword">,</span><span class="string">&apos;\\n&apos;</span><span class="keyword">); </span><span class="comment">//same match - 3 backslashes are interpreted as 2 in PHP, if the following character is not escapeable<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
There does not seem to be any mention of the PHP version of switches that can be used with regular expressions.
<br>
<br>preg_match_all(&apos;/regular expr/sim&apos;,$text).
<br>
<br>The s i m being the location for and available switches (I know about)
<br>The i is to ignore letter cases (this is commonly known - I think)
<br>The s tells the code NOT TO stop searching when it encounters \n (line break) - this is important with multi-line entries for example text from an editor that needs search.
<br>The m tells the code it is a multi-line entry, but importantly allows the use of ^ and $ to work when showing start and end.
<br>
<br>I am hoping this will save someone from the 4 hours of torture that I endured, trying to workout this issue.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Bugs of preg_match (PHP-version 5.2.5)<br><br>In most cases, the following example will show one of two PHP-bugs discovered with preg_match depending on your PHP-version and configuration.<br><br><span class="default">&lt;?php<br><br>$text </span><span class="keyword">= </span><span class="string">&quot;test=&quot;</span><span class="keyword">;<br></span><span class="comment">// creates a rather long text<br></span><span class="keyword">for (</span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++ &lt; </span><span class="default">100000</span><span class="keyword">;)<br>&#xA0; &#xA0; </span><span class="default">$text </span><span class="keyword">.= </span><span class="string">&quot;%AB&quot;</span><span class="keyword">;<br><br></span><span class="comment">// a typical URL_query validity-checker (the pattern&apos;s function does not matter for this example)<br></span><span class="default">$pattern&#xA0; &#xA0; </span><span class="keyword">= </span><span class="string">&apos;/^(?:[;\/?:@&amp;=+$,]|(?:[^\W_]|[-_.!~*\()\[\] ])|(?:%[\da-fA-F]{2}))*$/&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; <br></span><span class="default">var_dump</span><span class="keyword">( </span><span class="default">preg_match</span><span class="keyword">( </span><span class="default">$pattern</span><span class="keyword">, </span><span class="default">$text </span><span class="keyword">) );<br><br></span><span class="default">?&gt;<br></span><br>Possible bug (1):<br>=============<br>On one of our Linux-Servers the above example crashes PHP-execution with a C(?) Segmentation Fault(!). This seems to be a known bug (see <a href="http://bugs.php.net/bug.php?id=40909" rel="nofollow" target="_blank">http://bugs.php.net/bug.php?id=40909</a>), but I don&apos;t know if it has been fixed, yet.<br>If you are looking for a work-around, the following code-snippet is what I found helpful. It wraps the possibly crashing preg_match call by decreasing the PCRE recursion limit in order to result in a Reg-Exp error instead of a PHP-crash.<br><br><span class="default">&lt;?php<br></span><span class="keyword">[...]<br><br></span><span class="comment">// decrease the PCRE recursion limit for the (possibly dangerous) preg_match call<br></span><span class="default">$former_recursion_limit </span><span class="keyword">= </span><span class="default">ini_set</span><span class="keyword">( </span><span class="string">&quot;pcre.recursion_limit&quot;</span><span class="keyword">, </span><span class="default">10000 </span><span class="keyword">);<br><br></span><span class="comment">// the wrapped preg_match call<br></span><span class="default">$result </span><span class="keyword">= </span><span class="default">preg_match</span><span class="keyword">( </span><span class="default">$pattern</span><span class="keyword">, </span><span class="default">$text </span><span class="keyword">);<br><br></span><span class="comment">// reset the PCRE recursion limit to its original value<br></span><span class="default">ini_set</span><span class="keyword">( </span><span class="string">&quot;pcre.recursion_limit&quot;</span><span class="keyword">, </span><span class="default">$former_recursion_limit </span><span class="keyword">);<br><br></span><span class="comment">// if the reg-exp fails due to the decreased recursion limit we may not make any statement, but PHP-execution continues<br></span><span class="keyword">if ( </span><span class="default">PREG_RECURSION_LIMIT_ERROR </span><span class="keyword">=== </span><span class="default">preg_last_error</span><span class="keyword">() )<br>{<br>&#xA0; &#xA0; </span><span class="comment">// react on the failed regular expression here<br>&#xA0; &#xA0; </span><span class="default">$result </span><span class="keyword">= [...];<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; </span><span class="comment">// do logging or email-sending here<br>&#xA0; &#xA0; </span><span class="keyword">[...]<br>} </span><span class="comment">//if<br><br></span><span class="default">?&gt;<br></span><br>Possible bug (2):<br>=============<br>On one of our Windows-Servers the above example does not crash PHP, but (directly) hits the recursion-limit. Here, the problem is that preg_match does not return boolean(false) as expected by the description / manual of above.<br>In short, preg_match seems to return an int(0) instead of the expected boolean(false) if the regular expression could not be executed due to the PCRE recursion-limit. So, if preg_match results in int(0) you seem to have to check preg_last_error() if maybe an error occurred.</span>
</div>
  

#


<div class="phpcode"><span class="html">
This sample is for checking persian character:<br><br><span class="default">&lt;?php<br>&#xA0;&#xA0; preg_match</span><span class="keyword">(</span><span class="string">&quot;/[\x{0600}-\x{06FF}\x]{1,32}/u&quot;</span><span class="keyword">, </span><span class="string">&apos;&#x645;&#x62D;&#x645;&#x62F;&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Because making a truly correct email validation function is harder than one may think, consider using this one which comes with PHP through the filter_var function (<a href="http://www.php.net/manual/en/function.filter-var.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/function.filter-var.php</a>):<br><br><span class="default">&lt;?php<br>$email </span><span class="keyword">= </span><span class="string">&quot;someone@domain .local&quot;</span><span class="keyword">;<br><br>if(!</span><span class="default">filter_var</span><span class="keyword">(</span><span class="default">$email</span><span class="keyword">, </span><span class="default">FILTER_VALIDATE_EMAIL</span><span class="keyword">)) {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;E-mail is not valid&quot;</span><span class="keyword">;<br>} else {<br>&#xA0; &#xA0; echo </span><span class="string">&quot;E-mail is valid&quot;</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
To validate directorys on Windows i used this:<br><br>if( preg_match(&quot;#^([a-z]{1}\:{1})?[\\\/]?([\-\w]+[\\\/]?)*$#i&quot;,$_GET[&apos;path&apos;],$matches) !== 1 ){<br>&#xA0; &#xA0; echo(&quot;Invalid value&quot;);<br>}else{<br>&#xA0; &#xA0; echo(&quot;Valid value&quot;);<br>}<br><br>The parts are:<br><br>#^ and $i&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; Make the string matches at all the pattern, from start to end for ensure a complete match.<br>([a-z]{1}\:{1})?&#xA0; &#xA0; &#xA0; &#xA0; The string may starts with one letter and a colon, but only 1 character for eachone, this is for the drive letter (C:)<br>[\\\/]?&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; The string may contain, but not require 1 slash or backslash after the drive letter, (\/)<br>([\-\w]+[\\\/]?)*&#xA0; &#xA0; The string must have 1 or more of any character like hyphen, letter, number, underscore, and may contain a slash or back slash at the end, to have a directory like (&quot;/&quot; or &quot;folderName&quot; or &quot;folderName/&quot;), this may be repeated one or more times.</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you want to validate an email in one line, use filter_var() function !<br><a href="http://fr.php.net/manual/en/function.filter-var.php" rel="nofollow" target="_blank">http://fr.php.net/manual/en/function.filter-var.php</a><br><br>easy use, as described in the document example :<br>var_dump(filter_var(&apos;bob@example.com&apos;, FILTER_VALIDATE_EMAIL));</span>
</div>
  

#


<div class="phpcode"><span class="html">
As I wasted lots of time finding a REAL regex for URLs and resulted in building it on my own, I now have found one, that seems to work for all kinds of urls:
<br>
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0; $regex </span><span class="keyword">= </span><span class="string">&quot;((https?|ftp)\:\/\/)?&quot;</span><span class="keyword">; </span><span class="comment">// SCHEME
<br>&#xA0; &#xA0; </span><span class="default">$regex </span><span class="keyword">.= </span><span class="string">&quot;([a-z0-9+!*(),;?&amp;=\$_.-]+(\:[a-z0-9+!*(),;?&amp;=\$_.-]+)?@)?&quot;</span><span class="keyword">; </span><span class="comment">// User and Pass
<br>&#xA0; &#xA0; </span><span class="default">$regex </span><span class="keyword">.= </span><span class="string">&quot;([a-z0-9-.]*)\.([a-z]{2,3})&quot;</span><span class="keyword">; </span><span class="comment">// Host or IP
<br>&#xA0; &#xA0; </span><span class="default">$regex </span><span class="keyword">.= </span><span class="string">&quot;(\:[0-9]{2,5})?&quot;</span><span class="keyword">; </span><span class="comment">// Port
<br>&#xA0; &#xA0; </span><span class="default">$regex </span><span class="keyword">.= </span><span class="string">&quot;(\/([a-z0-9+\$_-]\.?)+)*\/?&quot;</span><span class="keyword">; </span><span class="comment">// Path
<br>&#xA0; &#xA0; </span><span class="default">$regex </span><span class="keyword">.= </span><span class="string">&quot;(\?[a-z+&amp;\$_.-][a-z0-9;:@&amp;%=+\/\$_.-]*)?&quot;</span><span class="keyword">; </span><span class="comment">// GET Query
<br>&#xA0; &#xA0; </span><span class="default">$regex </span><span class="keyword">.= </span><span class="string">&quot;(#[a-z_.-][a-z0-9+\$_.-]*)?&quot;</span><span class="keyword">; </span><span class="comment">// Anchor
<br></span><span class="default">?&gt;
<br></span>
<br>Then, the correct way to check against the regex ist as follows:
<br>
<br><span class="default">&lt;?php
<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">if(</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&quot;/^</span><span class="default">$regex</span><span class="string">$/&quot;</span><span class="keyword">, </span><span class="default">$url</span><span class="keyword">))
<br>&#xA0; &#xA0; &#xA0;&#xA0; {
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; return </span><span class="default">true</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0;&#xA0; }
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I see a lot of people trying to put together phone regex&apos;s and struggling (hey, no worries...they&apos;re complicated). Here&apos;s one that we use that&apos;s pretty nifty. It&apos;s not perfect, but it should work for most non-idealists.
<br>
<br>*** Note: Only matches U.S. phone numbers. ***
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="comment">// all on one line...
<br></span><span class="default">$regex </span><span class="keyword">= </span><span class="string">&apos;/^(?:1(?:[. -])?)?(?:\((?=\d{3}\)))?([2-9]\d{2})(?:(?&lt;=\(\d{3})\))? ?(?:(?&lt;=\d{3})[.-])?([2-9]\d{2})[. -]?(\d{4})(?: (?i:ext)\.? ?(\d{1,5}))?$/&apos;</span><span class="keyword">;
<br>
<br></span><span class="comment">// or broken up
<br></span><span class="default">$regex </span><span class="keyword">= </span><span class="string">&apos;/^(?:1(?:[. -])?)?(?:\((?=\d{3}\)))?([2-9]\d{2})&apos;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">.</span><span class="string">&apos;(?:(?&lt;=\(\d{3})\))? ?(?:(?&lt;=\d{3})[.-])?([2-9]\d{2})&apos;
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">.</span><span class="string">&apos;[. -]?(\d{4})(?: (?i:ext)\.? ?(\d{1,5}))?$/&apos;</span><span class="keyword">;
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>If you&apos;re wondering why all the non-capturing subpatterns (which look like this &quot;(?:&quot;, it&apos;s so that we can do this:
<br>
<br><span class="default">&lt;?php
<br>
<br>$formatted </span><span class="keyword">= </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="default">$regex</span><span class="keyword">, </span><span class="string">&apos;($1) $2-$3 ext. $4&apos;</span><span class="keyword">, </span><span class="default">$phoneNumber</span><span class="keyword">);
<br>
<br></span><span class="comment">// or, provided you use the $matches argument in preg_match
<br>
<br></span><span class="default">$formatted </span><span class="keyword">= </span><span class="string">&quot;(</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">1</span><span class="keyword">]</span><span class="string">) </span><span class="default">$matches</span><span class="keyword">[</span><span class="default">2</span><span class="keyword">]</span><span class="string">-</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">3</span><span class="keyword">]</span><span class="string">&quot;</span><span class="keyword">;
<br>if (</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">4</span><span class="keyword">]) </span><span class="default">$formatted </span><span class="keyword">.= </span><span class="string">&quot; </span><span class="default">$matches</span><span class="keyword">[</span><span class="default">4</span><span class="keyword">]</span><span class="string">&quot;</span><span class="keyword">;
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>*** Results: ***
<br>520-555-5542 :: MATCH
<br>520.555.5542 :: MATCH
<br>5205555542 :: MATCH
<br>520 555 5542 :: MATCH
<br>520) 555-5542 :: FAIL
<br>(520 555-5542 :: FAIL
<br>(520)555-5542 :: MATCH
<br>(520) 555-5542 :: MATCH
<br>(520) 555 5542 :: MATCH
<br>520-555.5542 :: MATCH
<br>520 555-0555 :: MATCH
<br>(520)5555542 :: MATCH
<br>520.555-4523 :: MATCH
<br>19991114444 :: FAIL
<br>19995554444 :: MATCH
<br>514 555 1231 :: MATCH
<br>1 555 555 5555 :: MATCH
<br>1.555.555.5555 :: MATCH
<br>1-555-555-5555 :: MATCH
<br>520-555-5542 ext.123 :: MATCH
<br>520.555.5542 EXT 123 :: MATCH
<br>5205555542 Ext. 7712 :: MATCH
<br>520 555 5542 ext 5 :: MATCH
<br>520) 555-5542 :: FAIL
<br>(520 555-5542 :: FAIL
<br>(520)555-5542 ext .4 :: FAIL
<br>(512) 555-1234 ext. 123 :: MATCH
<br>1(555)555-5555 :: MATCH</span>
</div>
  

#


<div class="phpcode"><span class="html">
Some times a Hacker use a php file or shell as a image to hack your website. so if you try to use move_uploaded_file() function as in example to allow for users to upload files, you must check if this file contains a bad codes or not so we use this function. preg match<br><br>in this function we use<br><br>unlink() - <a href="http://php.net/unlink" rel="nofollow" target="_blank">http://php.net/unlink</a><br><br>after you upload file check a file with below function. <br><br><span class="default">&lt;?php<br><br></span><span class="comment">/**<br> * A simple function to check file from bad codes.<br> *<br> * @param (string) $file - file path.<br> * @author Yousef Ismaeil - Cliprz[at]gmail[dot]com.<br> */<br></span><span class="keyword">function </span><span class="default">is_clean_file </span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; if (</span><span class="default">file_exists</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$contents </span><span class="keyword">= </span><span class="default">file_get_contents</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; else<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; exit(</span><span class="default">$file</span><span class="keyword">.</span><span class="string">&quot; Not exists.&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; if (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/(base64_|eval|system|shell_|exec|php_)/i&apos;</span><span class="keyword">,</span><span class="default">$contents</span><span class="keyword">))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; else if (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&quot;#&amp;\#x([0-9a-f]+);#i&quot;</span><span class="keyword">, </span><span class="default">$contents</span><span class="keyword">))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; elseif (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;#&amp;\#([0-9]+);#i&apos;</span><span class="keyword">, </span><span class="default">$contents</span><span class="keyword">))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; elseif (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&quot;#([a-z]*)=([\`\&apos;\&quot;]*)script:#iU&quot;</span><span class="keyword">, </span><span class="default">$contents</span><span class="keyword">))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; elseif (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&quot;#([a-z]*)=([\`\&apos;\&quot;]*)javascript:#iU&quot;</span><span class="keyword">, </span><span class="default">$contents</span><span class="keyword">))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; elseif (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&quot;#([a-z]*)=([\&apos;\&quot;]*)vbscript:#iU&quot;</span><span class="keyword">, </span><span class="default">$contents</span><span class="keyword">))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; elseif (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&quot;#(&lt;[^&gt;]+)style=([\`\&apos;\&quot;]*).*expression\([^&gt;]*&gt;#iU&quot;</span><span class="keyword">, </span><span class="default">$contents</span><span class="keyword">))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; elseif (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&quot;#(&lt;[^&gt;]+)style=([\`\&apos;\&quot;]*).*behaviour\([^&gt;]*&gt;#iU&quot;</span><span class="keyword">, </span><span class="default">$contents</span><span class="keyword">))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; elseif (</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&quot;#&lt;/*(applet|link|style|script|iframe|frame|frameset|html|body|title|div|p|form)[^&gt;]*&gt;#i&quot;</span><span class="keyword">, </span><span class="default">$contents</span><span class="keyword">))<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">true</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; else<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">false</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;<br></span><br>Use<br><br><span class="default">&lt;?php<br></span><span class="comment">// If image contains a bad codes<br></span><span class="default">$image&#xA0;&#xA0; </span><span class="keyword">= </span><span class="string">&quot;simpleimage.png&quot;</span><span class="keyword">;<br><br>if (</span><span class="default">is_clean_file</span><span class="keyword">(</span><span class="default">$image</span><span class="keyword">))<br>{<br>&#xA0; &#xA0; echo </span><span class="string">&quot;Bad codes this is not image&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; </span><span class="default">unlink</span><span class="keyword">(</span><span class="default">$image</span><span class="keyword">);<br>}<br>else<br>{<br>&#xA0; &#xA0; echo </span><span class="string">&quot;This is a real image.&quot;</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
for those coming over from ereg, preg_match can be quite intimidating. to get started here is a migration tip.
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">if(</span><span class="default">ereg</span><span class="keyword">(</span><span class="string">&apos;[^0-9A-Za-z]&apos;</span><span class="keyword">,</span><span class="default">$test_string</span><span class="keyword">)) </span><span class="comment">// will be true if characters arnt 0-9, A-Z or a-z.
<br>
<br></span><span class="keyword">if(</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;/[^0-9A-Za-z]/&apos;</span><span class="keyword">,</span><span class="default">$test_string</span><span class="keyword">)) </span><span class="comment">// this is the preg_match version. the /&apos;s are now required.
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.preg-match.php)

**[⬆ to root](/)**