# mysqli::$insert_id




<div class="phpcode"><span class="html">
I have received many statements that the insert_id property has a bug because it &quot;works sometimes&quot;.&#xA0; Keep in mind that when using the OOP approach, the actual instantiation of the mysqli class will hold the insert_id.&#xA0; <br><br>The following code will return nothing.<br><span class="default">&lt;?php<br>$mysqli </span><span class="keyword">= new </span><span class="default">mysqli</span><span class="keyword">(</span><span class="string">&apos;host&apos;</span><span class="keyword">,</span><span class="string">&apos;user&apos;</span><span class="keyword">,</span><span class="string">&apos;pass&apos;</span><span class="keyword">,</span><span class="string">&apos;db&apos;</span><span class="keyword">);<br>if (</span><span class="default">$result </span><span class="keyword">= </span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;INSERT INTO t (field) VALUES (&apos;value&apos;);&quot;</span><span class="keyword">)) {<br>&#xA0;&#xA0; echo </span><span class="string">&apos;The ID is: &apos;</span><span class="keyword">.</span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">insert_id</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;<br></span><br>This is because the insert_id property doesn&apos;t belong to the result, but rather the actual mysqli class.&#xA0; This would work:<br><br><span class="default">&lt;?php<br>$mysqli </span><span class="keyword">= new </span><span class="default">mysqli</span><span class="keyword">(</span><span class="string">&apos;host&apos;</span><span class="keyword">,</span><span class="string">&apos;user&apos;</span><span class="keyword">,</span><span class="string">&apos;pass&apos;</span><span class="keyword">,</span><span class="string">&apos;db&apos;</span><span class="keyword">);<br>if (</span><span class="default">$result </span><span class="keyword">= </span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;INSERT INTO t (field) VALUES (&apos;value&apos;);&quot;</span><span class="keyword">)) {<br>&#xA0;&#xA0; echo </span><span class="string">&apos;The ID is: &apos;</span><span class="keyword">.</span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">insert_id</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.insert-id.php)

**[⬆ to root](/)**