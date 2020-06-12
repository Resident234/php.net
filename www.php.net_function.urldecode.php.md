# urldecode




<div class="phpcode"><span class="html">
When the client send Get data, utf-8 character encoding have a tiny problem with the urlencode.
<br>Consider the &quot;&#xBA;&quot; character. 
<br>Some clients can send (as example)
<br>foo.php?myvar=%BA
<br>and another clients send
<br>foo.php?myvar=%C2%BA (The &quot;right&quot; url encoding)
<br>
<br>in this scenary, you assign the value into variable $x
<br>
<br><span class="default">&lt;?php
<br>$x </span><span class="keyword">= </span><span class="default">$_GET</span><span class="keyword">[</span><span class="string">&apos;myvar&apos;</span><span class="keyword">];
<br></span><span class="default">?&gt;
<br></span>
<br>$x store: in the first case &quot;&#xFFFD;&quot; (bad) and in the second case &quot;&#xBA;&quot; (good)
<br>
<br>To fix that, you can use this function:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">function </span><span class="default">to_utf8</span><span class="keyword">( </span><span class="default">$string </span><span class="keyword">) {
<br></span><span class="comment">// From <a href="http://w3.org/International/questions/qa-forms-utf-8.html" rel="nofollow" target="_blank">http://w3.org/International/questions/qa-forms-utf-8.html</a>
<br>&#xA0; &#xA0; </span><span class="keyword">if ( </span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&apos;%^(?:
<br>&#xA0; &#xA0; &#xA0; [\x09\x0A\x0D\x20-\x7E]&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; # ASCII
<br>&#xA0; &#xA0; | [\xC2-\xDF][\x80-\xBF]&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; # non-overlong 2-byte
<br>&#xA0; &#xA0; | \xE0[\xA0-\xBF][\x80-\xBF]&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; # excluding overlongs
<br>&#xA0; &#xA0; | [\xE1-\xEC\xEE\xEF][\x80-\xBF]{2}&#xA0; # straight 3-byte
<br>&#xA0; &#xA0; | \xED[\x80-\x9F][\x80-\xBF]&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; # excluding surrogates
<br>&#xA0; &#xA0; | \xF0[\x90-\xBF][\x80-\xBF]{2}&#xA0; &#xA0; &#xA0; # planes 1-3
<br>&#xA0; &#xA0; | [\xF1-\xF3][\x80-\xBF]{3}&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; # planes 4-15
<br>&#xA0; &#xA0; | \xF4[\x80-\x8F][\x80-\xBF]{2}&#xA0; &#xA0; &#xA0; # plane 16
<br>)*$%xs&apos;</span><span class="keyword">, </span><span class="default">$string</span><span class="keyword">) ) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$string</span><span class="keyword">;
<br>&#xA0; &#xA0; } else {
<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">iconv</span><span class="keyword">( </span><span class="string">&apos;CP1252&apos;</span><span class="keyword">, </span><span class="string">&apos;UTF-8&apos;</span><span class="keyword">, </span><span class="default">$string</span><span class="keyword">);
<br>&#xA0; &#xA0; }
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>and assign in this way:
<br>
<br><span class="default">&lt;?php
<br>$x </span><span class="keyword">= </span><span class="default">to_utf8</span><span class="keyword">( </span><span class="default">$_GET</span><span class="keyword">[</span><span class="string">&apos;myvar&apos;</span><span class="keyword">] );
<br></span><span class="default">?&gt;
<br></span>
<br>$x store: in the first case &quot;&#xBA;&quot; (good) and in the second case &quot;&#xBA;&quot; (good)
<br>
<br>Solve a lot of i18n problems.
<br>
<br>Please fix the auto-urldecode of $_GET var in the next PHP version.
<br>
<br>Bye.
<br>
<br>Alejandro Salamanca</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you are escaping strings in javascript and want to decode them in PHP with urldecode (or want PHP to decode them automatically when you&apos;re putting them in the query string or post request), you should use the javascript function encodeURIComponent() instead of escape(). Then you won&apos;t need any of the fancy custom utf_urldecode functions from the previous comments.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.urldecode.php)

**[⬆ to root](/)**