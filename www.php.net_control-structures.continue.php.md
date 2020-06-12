# continue




<div class="phpcode"><span class="html">
The remark &quot;in PHP the switch statement is considered a looping structure for the purposes of continue&quot; near the top of this page threw me off, so I experimented a little using the following code to figure out what the exact semantics of continue inside a switch is:<br><br><span class="default">&lt;?php<br><br>&#xA0; &#xA0; </span><span class="keyword">for( </span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">3</span><span class="keyword">; ++ </span><span class="default">$i </span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos; [&apos;</span><span class="keyword">, </span><span class="default">$i</span><span class="keyword">, </span><span class="string">&apos;] &apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; switch( </span><span class="default">$i </span><span class="keyword">)<br>&#xA0; &#xA0; &#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">0</span><span class="keyword">: echo </span><span class="string">&apos;zero&apos;</span><span class="keyword">; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">1</span><span class="keyword">: echo </span><span class="string">&apos;one&apos; </span><span class="keyword">; </span><span class="default">XXXX</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case </span><span class="default">2</span><span class="keyword">: echo </span><span class="string">&apos;two&apos; </span><span class="keyword">; break;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos; &lt;&apos; </span><span class="keyword">, </span><span class="default">$i</span><span class="keyword">, </span><span class="string">&apos;&gt; &apos;</span><span class="keyword">;<br>&#xA0; &#xA0; }<br><br></span><span class="default">?&gt;<br></span><br>For XXXX I filled in<br><br>- continue 1<br>- continue 2<br>- break 1<br>- break 2<br><br>and observed the different results.&#xA0; This made me come up with the following one-liner that describes the difference between break and continue:<br><br>continue resumes execution just before the closing curly bracket ( } ), and break resumes execution just after the closing curly bracket.<br><br>Corollary: since a switch is not (really) a looping structure, resuming execution just before a switch&apos;s closing curly bracket has the same effect as using a break statement.&#xA0; In the case of (for, while, do-while) loops, resuming execution just prior their closing curly brackets means that a new iteration is started --which is of course very unlike the behavior of a break statement.<br><br>In the one-liner above I ignored the existence of parameters to break/continue, but the one-liner is also valid when parameters are supplied.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Using continue and break:
<br>
<br><span class="default">&lt;?php
<br>$stack </span><span class="keyword">= array(</span><span class="string">&apos;first&apos;</span><span class="keyword">, </span><span class="string">&apos;second&apos;</span><span class="keyword">, </span><span class="string">&apos;third&apos;</span><span class="keyword">, </span><span class="string">&apos;fourth&apos;</span><span class="keyword">, </span><span class="string">&apos;fifth&apos;</span><span class="keyword">);
<br>
<br>foreach(</span><span class="default">$stack </span><span class="keyword">AS </span><span class="default">$v</span><span class="keyword">){
<br>&#xA0; &#xA0; if(</span><span class="default">$v </span><span class="keyword">== </span><span class="string">&apos;second&apos;</span><span class="keyword">)continue;
<br>&#xA0; &#xA0; if(</span><span class="default">$v </span><span class="keyword">== </span><span class="string">&apos;fourth&apos;</span><span class="keyword">)break;
<br>&#xA0; &#xA0; echo </span><span class="default">$v</span><span class="keyword">.</span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">;
<br>}
<br></span><span class="comment">/*
<br>
<br>first
<br>third
<br>
<br>*/
<br>
<br></span><span class="default">$stack2 </span><span class="keyword">= array(</span><span class="string">&apos;one&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;first&apos;</span><span class="keyword">, </span><span class="string">&apos;two&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;second&apos;</span><span class="keyword">, </span><span class="string">&apos;three&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;third&apos;</span><span class="keyword">, </span><span class="string">&apos;four&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;fourth&apos;</span><span class="keyword">, </span><span class="string">&apos;five&apos;</span><span class="keyword">=&gt;</span><span class="string">&apos;fifth&apos;</span><span class="keyword">);
<br>foreach(</span><span class="default">$stack2 </span><span class="keyword">AS </span><span class="default">$k</span><span class="keyword">=&gt;</span><span class="default">$v</span><span class="keyword">){
<br>&#xA0; &#xA0; if(</span><span class="default">$v </span><span class="keyword">== </span><span class="string">&apos;second&apos;</span><span class="keyword">)continue;
<br>&#xA0; &#xA0; if(</span><span class="default">$k </span><span class="keyword">== </span><span class="string">&apos;three&apos;</span><span class="keyword">)continue;
<br>&#xA0; &#xA0; if(</span><span class="default">$v </span><span class="keyword">== </span><span class="string">&apos;fifth&apos;</span><span class="keyword">)break;
<br>&#xA0; &#xA0; echo </span><span class="default">$k</span><span class="keyword">.</span><span class="string">&apos; ::: &apos;</span><span class="keyword">.</span><span class="default">$v</span><span class="keyword">.</span><span class="string">&apos;&lt;br&gt;&apos;</span><span class="keyword">;
<br>}
<br></span><span class="comment">/*
<br>
<br>one ::: first
<br>four ::: fourth
<br>
<br>*/
<br>
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you use a incrementing value in your loop, be sure to increment it before calling continue; or you might get an infinite loop.</span>
</div>
  

#


<div class="phpcode"><span class="html">
The most basic example that print &quot;13&quot;, skipping over 2.<br><br><span class="default">&lt;?php<br>$arr </span><span class="keyword">= array(</span><span class="default">1</span><span class="keyword">, </span><span class="default">2</span><span class="keyword">, </span><span class="default">3</span><span class="keyword">);<br>foreach(</span><span class="default">$arr </span><span class="keyword">as </span><span class="default">$number</span><span class="keyword">) {<br>&#xA0; if(</span><span class="default">$number </span><span class="keyword">== </span><span class="default">2</span><span class="keyword">) {<br>&#xA0; &#xA0; continue;<br>&#xA0; }<br>&#xA0; print </span><span class="default">$number</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.continue.php)

**[To root](/)**