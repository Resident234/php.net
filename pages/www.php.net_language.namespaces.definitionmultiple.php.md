# Defining multiple namespaces in the same file




<div class="phpcode"><span class="html">
using of global namespaces and multiple namespaces in one PHP file increase the complexity and decrease readability of the code.<br>Let&apos;s try not use this scheme even it&apos;s very necessary (although there is not)</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br><br></span><span class="comment">// You cannot mix bracketed namespace declarations with unbracketed namespace declarations - will result in a Fatal error<br><br></span><span class="keyword">namespace </span><span class="default">a</span><span class="keyword">;<br><br>echo </span><span class="string">&quot;I belong to namespace a&quot;</span><span class="keyword">;<br><br>namespace </span><span class="default">b </span><span class="keyword">{<br>&#xA0; &#xA0; echo </span><span class="string">&quot;I&apos;m from namespace b&quot;</span><span class="keyword">;<br>}</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
<span class="default">&lt;?php<br></span><span class="comment">//Namespace can be used in this way also<br></span><span class="keyword">namespace </span><span class="default">MyProject </span><span class="keyword">{<br><br>function </span><span class="default">connect</span><span class="keyword">() { echo </span><span class="string">&quot;ONE&quot;</span><span class="keyword">;&#xA0; }<br>&#xA0; &#xA0; </span><span class="default">Sub</span><span class="keyword">\</span><span class="default">Level</span><span class="keyword">\</span><span class="default">connect</span><span class="keyword">();<br>}<br><br>namespace </span><span class="default">MyProject</span><span class="keyword">\</span><span class="default">Sub </span><span class="keyword">{<br>&#xA0; &#xA0; <br>function </span><span class="default">connect</span><span class="keyword">() { echo </span><span class="string">&quot;TWO&quot;</span><span class="keyword">;&#xA0; }<br>&#xA0; &#xA0; </span><span class="default">Level</span><span class="keyword">\</span><span class="default">connect</span><span class="keyword">();<br>}<br><br>namespace </span><span class="default">MyProject</span><span class="keyword">\</span><span class="default">Sub</span><span class="keyword">\</span><span class="default">Level </span><span class="keyword">{<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; function </span><span class="default">connect</span><span class="keyword">() { echo </span><span class="string">&quot;THREE&quot;</span><span class="keyword">;&#xA0; }&#xA0; &#xA0; <br>&#xA0; &#xA0; \</span><span class="default">MyProject</span><span class="keyword">\</span><span class="default">Sub</span><span class="keyword">\</span><span class="default">Level</span><span class="keyword">\</span><span class="default">connect</span><span class="keyword">(); </span><span class="comment">// OR we can use this as below<br>&#xA0; &#xA0; </span><span class="default">connect</span><span class="keyword">();<br>}</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.definitionmultiple.php)

**[To root](/README.md)**